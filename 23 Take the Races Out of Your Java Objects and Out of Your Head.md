# Take the Races Out of Your Java Objects, and Out of Your Head

*Originally published on LinkedIn: <https://lnkd.in/p/gQsHBWEq>*

An earlier post in this series described **exposed concurrency**: code whose correctness rests on someone reasoning about visibility, ordering, ownership, and interleaving — where an ordinary change, made by someone who never thought they were doing concurrency work, puts all of that reasoning back on the table. It ended on an open question: whether architecture can move part of that burden off the ordinary change.

This post is one answer, scoped to a single process. Java concurrency is usually treated as a coding problem: the right keyword, in the right place, every time somebody touches the class. Here it is treated as an architecture problem instead — one decision, made once, in one file.

Here is the whole idea, in two lines.

```java
// Java
int sum = adder.add(a, b);

// Lemina
int sum = lePost.call("adder#add", a, b);
```

One line of setup registers the object under a text name:

```java
lePost.register("adder", new Adder());
```

`Adder` is a plain class. No base class, no interface, no annotation, no generated anything. It has a public `add` method, which is all the name `adder#add` requires.

Lemina is the addressing model behind that second line; the last section says where it comes from. `LePost` is the carrier that holds the names, and you can build it in an afternoon.

That is the entire change to how you reach an object. What follows is why it is worth making.

## What a Java call actually is

Decompose an ordinary call site. There is a **caller**, which is a method invocation statement — not an object, a statement. And there is a **callee**, a method on a target object.

The statement requires three things:

1. **A held reference.** You must already have the object.
2. **Direct dispatch.** Nothing sits between you and it.
3. **A guaranteed return.** Control comes back to you when the callee finishes.

And it does one thing people rarely say out loud: **a Java call hands over your thread.** `adder.add(a, b)` does not ask the adder to do work. It runs the adder's code on *your* thread. Control transfer and thread identity are fused in a single statement.

That fusion is not a problem when one thread is running. Nothing in the syntax changes when a second one arrives.

## Where conflicts happen

Multi-threading conflicts happen inside the callee object.

That is not a definition, just where the state is. Two threads conflict where they meet, and they meet inside the object they both reach into. 64 threads executing one invocation statement means 64 threads inside one object.

Here is a plain object:

```java
class Tally {
    private int count;                     // an ordinary field
    // an ordinary read-modify-write
    public void add(int n) { count += n; }
    public int total() { return count; }
}
```

64 threads, 1,000 increments each, reached the Java way — `tally.add(1)`. Three typical runs:

```
expected = 64000    actual = 20323
expected = 64000    actual = 21308
expected = 64000    actual = 25266
```

Nobody is surprised. The usual answers are to synchronize `add`, or make `count` an `AtomicInteger`, or wrap the whole thing in a lock — and then to keep making that judgment every time somebody touches the class.

## What changes when you call by name

`call` is still an imperative statement. It blocks, it returns a value, and it goes where `tally.add(1)` went — the control flow you write is unchanged. What changes is that your thread never enters `Tally`. It waits at the carrier while that object's own thread runs the code and hands the answer back.

The sender no longer donates its thread. And that means the decision about which threads are allowed inside a given object stops being distributed across every call site and becomes **one decision, in one file**: the carrier's thread policy.

Register `Tally` under a name, reach it with `lePost.call("tally#add", 1)`, and give every registered name — every **position** — its own thread. The same test gives:

```
expected = 64000    actual = 64000
```

`Tally` is unchanged. Still `private int count`. Still `count += n`. Still no `synchronized`, no `volatile`, no `AtomicInteger`.

Two more forms come later, when you notice you do not need to wait: `send` drops a message and returns immediately, and `sendWithReplyTo` asks for the answer to arrive as another message. Those are additions. `call` is the one that replaces what you already write.

## What the name costs

A string is not a symbol. `"adder#add"` is not checked at compile time, so a typo surfaces at run time rather than in the editor; the return type is whatever the call site claims it is, unchecked; and rename refactoring, find-usages, and autocomplete stop reaching through to the recipient. That is a real trade, not a rough edge — a check moves off the compiler and onto your tests.

Whether it is worth paying is a longer argument than this post makes. Later posts take up the pros and cons.

## The LePost

This is one carrier, written for this post rather than as a reference implementation — a proof of concept that walks the happy path and skips nearly everything else. It compiles on Java 21 and it is what produced the numbers above.

```java
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
import java.util.Map;
import java.util.concurrent.*;
import java.util.concurrent.atomic.AtomicInteger;

/**
 * An in-process Lemina carrier.
 *
 * Register plain objects under text names, then reach them by
 * name: call("adder#add", a, b) in place of adder.add(a, b).
 *
 * Thread policy: one position, one thread. Each registered
 * object gets its own thread and its own inbox. The thread
 * takes one message, runs it to completion, then takes the
 * next. The object never sees a second thread, so it needs no
 * synchronized, no volatile, no concurrent collections.
 *
 * A proof of concept: happy paths only. An error reaches a
 * sender that asked for a reply, and is dropped by a one-way
 * send.
 */
public final class LePost {

    private record Position(
            Object backing,
            BlockingQueue<Runnable> inbox,
            Thread worker) {}

    private final Map<String, Position> positions =
            new ConcurrentHashMap<>();

    // ----- registration -----

    public void register(String name, Object backing) {
        // replacing a position retires its old thread
        unregister(name);

        BlockingQueue<Runnable> inbox = new LinkedBlockingQueue<>();

        // One virtual thread per position: cheap enough for a
        // lot of positions. Pre-Java-21: new Thread(...) and
        // keep the count sane.
        Thread worker = Thread.ofVirtual()
                .name("lemina-" + name)
                .unstarted(() -> {
                    try {
                        // one message at a time, in arrival order
                        while (true) inbox.take().run();
                    } catch (InterruptedException unregistered) {
                        // the position is gone; the thread ends
                    }
                });

        positions.put(
                name, new Position(backing, inbox, worker));
        worker.start();
    }

    public void unregister(String name) {
        Position gone = positions.remove(name);
        if (gone != null) gone.worker().interrupt();
    }

    // ----- calling by name: what replaces a method call -----

    @SuppressWarnings("unchecked")
    public <T> T call(String to, Object... args) {
        Reply reply = new Reply();
        register(reply.name, reply);
        try {
            post(to, reply.address(), args);
            Object result = reply.future.get();
            if (result instanceof Throwable cause) {
                throw new RuntimeException(cause);
            }
            return (T) result;
        } catch (InterruptedException | ExecutionException e) {
            throw new RuntimeException(e);
        } finally {
            unregister(reply.name);
        }
    }

    /**
     * Public so the carrier can reach onResult by name, like
     * any other backing object.
     */
    public static final class Reply {
        private static final AtomicInteger SEQUENCE =
                new AtomicInteger();

        private final String name =
                "reply-" + SEQUENCE.incrementAndGet();
        private final CompletableFuture<Object> future =
                new CompletableFuture<>();

        public void onResult(Object result) {
            future.complete(result);
        }

        private String address() {
            return name + "#onResult";
        }
    }

    // ----- one-way forms, added on top of the same delivery -----

    public void send(String to, Object... args) {
        post(to, null, args);
    }

    public void sendWithReplyTo(
            String to, String replyTo, Object... args) {
        post(to, replyTo, args);
    }

    // ----- carrying and handoff -----

    private void post(String to, String replyTo, Object[] args) {
        Position position = positions.get(localName(to));
        position.inbox().offer(() -> handoff(
                position, attention(to), replyTo, args));
    }

    private void handoff(
            Position position,
            String attention,
            String replyTo,
            Object[] args) {
        Object backing = position.backing();
        Object result;
        try {
            result = attentionMethod(backing, attention, args)
                    .invoke(backing, args);
        } catch (Exception e) {
            result = e instanceof InvocationTargetException wrapped
                    ? wrapped.getCause()
                    : e;
        }
        if (replyTo != null) {
            send(replyTo, result);
        }
    }

    private static Method attentionMethod(
            Object backing, String attention, Object[] args) {
        for (Method method : backing.getClass().getMethods()) {
            if (method.getName().equals(attention)
                    && method.getParameterCount() == args.length) {
                return method;
            }
        }
        throw new IllegalArgumentException(
                "no public %s(%d args) on %s".formatted(
                        attention,
                        args.length,
                        backing.getClass().getName()));
    }

    // ----- address: name#attention -----

    private static String localName(String address) {
        int hash = address.indexOf('#');
        return hash < 0 ? address : address.substring(0, hash);
    }

    private static String attention(String address) {
        int hash = address.indexOf('#');
        return hash < 0 ? null : address.substring(hash + 1);
    }
}
```

## Actor-style execution, ordinary objects

The execution policy is actor-like on purpose: one position, one thread, an inbox in front of it. If that reminds you of an actor system, it should. What comes out is application code with no locks in it — the object is reached by one thread at a time, in arrival order, and never needs `synchronized` or a concurrent collection to defend itself.

The part you cannot read off the code is memory visibility. `java.util.concurrent` documents a happens-before edge from putting an element into a `BlockingQueue` to taking that element out [1], so whatever a sender wrote before posting is visible to the recipient, and whatever the recipient wrote in message *n* is visible in message *n+1*. That is why `Tally` needs no `volatile`. Two of the hardest questions in a Java code review — *is this safely published?*, *is there a happens-before edge?* — are answered once, by the carrier, instead of by whoever writes the next diff.

One rule comes with it: **`call` from outside a position, `sendWithReplyTo` from inside one.** A position has one thread, and blocking it to wait on itself would stop everything.

What is not actor-like is the recipient. `Tally` extends nothing, implements nothing, is annotated with nothing, and knows nothing about the carrier. No mailbox type, no supervision tree, no framework to adopt. The address is a string. And the runtime is about 170 lines you own — including the part that decides which threads run your code, which is exactly the part a framework keeps for itself.

Build your own. The thread policy lives in `register`.

## Where this comes from

Lemina is the addressing model described in *"Addressing as an Overlay Without IDL: One Sender Surface from In-Process to Cross-Language"* [2], and the `LePost` above is that model applied to ordinary in-process Java. The model in full is specified separately [3]. That paper is about a larger claim — one sender surface that survives a move from in-process to cross-language — and this post deliberately takes none of it. Everything here stays inside a single process, where the only thing worth having is the one above: your thread stops at the carrier, and one file decides who runs your object.

---

## References

[1] Java Platform SE 21 API Documentation, "Memory Consistency Properties," `java.util.concurrent` package summary. Documents the happens-before edges the JDK guarantees, including the edge from putting an element into a `BlockingQueue` to taking that element out. https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/package-summary.html

[2] Rex Young, "Addressing as an Overlay Without IDL: One Sender Surface from In-Process to Cross-Language," 2026. Presents the addressing model this post applies to in-process Java: a textual address, one-way delivery through a mediating carrier, and recipients that need no interface, base class, or generated stub. https://doi.org/10.5281/zenodo.21232901 — paper and source code: https://github.com/young-rex/lemina/tree/main/papers/addressing-as-an-overlay-without-idl

[3] Rex Young, "Lemina Addressing Metamodel," 2026. The model in full — roles, address spaces, and the layered vocabulary this post deliberately leaves out. Relevant here for one thing: it leaves the carrier's delivery behavior open by design, which is why the thread policy is a choice a binding gets to make. https://doi.org/10.5281/zenodo.20680627 — metamodel and proof-of-concept code: https://github.com/young-rex/lemina/tree/main/model

## Earlier in this series

- [Exposed Concurrency: A Standing Obligation, Not Just a Hiring Requirement](Post 21)
