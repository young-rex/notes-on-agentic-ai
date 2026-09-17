# Lemina Is an Architectural Fix for Java's Concurrency

*Originally published on LinkedIn: <https://lnkd.in/p/gSxvSmfa>*

The previous post argued that Java's concurrency problems are structural: they come from where threads meet objects, not from any line of code [P25]. This post follows that diagnosis with a fix and grades what it achieves — and what it leaves behind.

An earlier post showed the fix working: a plain object registered under a text name, reached by that name instead of by reference, and a race disappeared without a keyword being added [P23]. There are two contributions in that result, and they are not the same kind of thing.

Lemina's addressing is infrastructure: a second way for a sender to reach an object, beside the reference. LePost is one in-process binding of it (which runs inside one JVM) and it implements one execution policy on that infrastructure: which thread runs the object. The addressing infrastructure does not prescribe an execution policy. Post 21 showed that confinement can also be implemented with a single-threaded executor [P21]. In LePost, addressed messages pass through the carrier, giving it a place to control execution without changing the recipient class.

The previous post listed five problems that follow from Java's model — races, visibility, locks that do not compose, isolation that is not enforced, and an obligation that does not expire [P25]. If they are structural, then a fix that moves where threads meet objects should clear some and leave the rest untouched. That is what happens.

To see what moved, recall the earlier post's decomposition of a Java call [P23]. **adder.add(a, b)** does two things in one statement: it transfers control to **add**, and it runs **add** on the caller's thread. Control transfer and thread identity are fused. That fusion is the fact the previous post built on — any thread that can reach an object can run it. Separating them takes two steps, and the first is not about threads at all.

## First contribution: a second way to reach an object

In Java an object has one way to reach another: hold a reference and invoke through it. The earlier post listed what that one way requires of the statement — a held reference, direct dispatch, a guaranteed return — and all three are properties of the only way there is [P23]. Every pattern that puts something between two objects — a proxy, an interface, an actor reference — is built out of references, and the thing in between is specific to the recipient: an interface it implements, a wrapper written for it, or a stub generated from one of those.

Lemina adds a second way. **lePost.call("adder#add", a, b)** reaches **Adder** by an address: a text name the carrier resolves to a registered object and a public method it already has. The sender holds no reference to the recipient; it holds the carrier, and the carrier holds the recipient. Nothing is dispatched directly. The return is still guaranteed, in the sense the earlier post meant — control comes back when the work finishes — which is what lets **call** stand in for the statement it replaces. Of the three requirements, the first two are gone and the third is kept. The sender still has to know the address and the arguments the method expects; reaching the object is no longer its business.

That is infrastructure, in the plain sense: a general mechanism that exists before any particular class, and that no class has to be written for. **Adder** has no base class, no interface, no annotation, and no declaration of how it is to be reached. It was registered, and that is all. The address is general in the way a reference is general — it can name any object — and it asks nothing of the recipient's side: no interface, no wrapper, no generated stub. Lemina specifies the address and the act of posting to it, and leaves delivery behavior open [5][6].

One consequence follows, and it is the one this post uses. Anything reached through infrastructure passes through it. A message addressed to **adder#add** arrives at the carrier before it arrives at **Adder**, so there is a point, outside both sender and recipient, where a decision about delivery can be made. A reference has no such point: **adder.add(a, b)** is on the caller's thread before anything could decide otherwise. Addressing does not make the decision. It makes a place for one.

## Second contribution: execution policy makes the decision

**LePost** fills that place with one policy: one position, one thread [P23]. **lePost.call("adder#add", a, b)** still blocks and returns the answer. Under this policy, the message waits in an inbox; a thread that belongs to that position takes it, runs **add**, and posts the answer back. The caller's thread never enters **Adder**.

So the object boundary is now an execution boundary, in the sense the previous post gave that term [P25]: how **Adder**'s code may be entered is decided at the carrier, by the policy in **register**. With all access routed through its one position, one thread — and only that thread — runs **Adder**. The boundary moved around an unchanged class.

The dependency is now explicit: **addressing is a second way to reach an object; that way passes through the carrier; the carrier chooses how the object is run; the chosen policy supplies serial execution and visibility.** The first three are infrastructure and hold under any policy. The fourth is **LePost**'s, and it is what the grades below measure. A different policy can use the same addresses and earn different grades.

## Five problems, graded

The previous post's list, in the same order, with a verdict on each. These grades assess **LePost**'s one-position-one-thread policy built on Lemina addressing. Every verdict rests on one condition: **the object is reached only through its one carrier position, and the mutable state protected by that position is accessed only there.** Register once, drop the direct reference, keep the protected mutable state within the position. Break the condition — a kept reference, a second registration — and the grades do not hold.

**Races — fixed**

A data race is two conflicting accesses to the same variable with no happens-before edge between them [P25]. **Tally**'s variable is **count**, and under the one-position-one-thread policy, every access to it that goes through the carrier is made by one thread. **count += n** is still three operations, and nothing can interleave with them, because there is nothing else on that thread and no other thread is admitted. The 64,000 in the earlier post is that verdict measured [P23].

**Visibility — fixed**

This is the verdict that cannot be read off the code, and it is the strongest one. The previous post explained *happens-before*: a write on one thread is guaranteed visible to another only across a formally defined edge, and the Java Memory Model lists which operations create one [P25]. The JDK documents one such edge for **BlockingQueue**: putting an element in happens-before taking it out [7]. **LePost** delivers every message through a **BlockingQueue**. So whatever the sender wrote before posting is visible to the position's thread when it takes the message; and whatever that thread wrote while handling message *n* is visible to itself when it handles *n+1*, because it is the same thread. The answer travels back over the same kind of edge, twice: the position's thread posts the result to the reply's own queue, and the reply completes a **Future**, for which the JDK documents the same guarantee — what the completing thread did happens-before the **get** that returns the result [7]. The caller sees the answer, and everything the position wrote before producing it. **Tally** needs no **volatile** for the same reason it needs no **synchronized**: the question was answered once, by the carrier's choice of queue, and the class never has to answer it.

**Locks do not compose — fixed inside, moved between**

Inside a position, there are no locks to compose. **Tally** has none. A class that calls another class on the same position has none. The composition problem the previous post described — two correctly locked classes combining into an incorrectly locked pair — cannot arise where there are no locks [P25].

Between positions, the problem changes form. **call** blocks the caller until the reply arrives. If position A **call**s position B while B is **call**ing A, both threads wait on inboxes neither is draining — the same lock-ordering deadlock in a new costume. The convention that prevents it (**call** from outside a position, **sendWithReplyTo** from inside one) is not enforced by the carrier [P23].

**Isolation is not enforced — moved, not fixed**

The carrier decides who enters the object. It does not decide what the object's arguments are. **call("tally#add", 1)** passes an **int**, which is a value. **call("ledger#post", entry)** passes a reference, and after the call both the sender and the ledger hold it. They are on different threads. Nothing stops either from mutating it, and the JVM heap is still one heap.

Access to the *object* is serialized; isolation of the *data crossing the boundary* is not. The systems that closed this gap — Erlang by copying [4], Swift by requiring **Sendable** at the boundary [8], Pony by making it a type [9] — did it by changing what crosses. **LePost** has nothing at that seam. Pass immutable values and it is safe; pass a mutable object and you are back in the model.

**The obligation does not expire — reduced**

Post 21 examined the cost of an obligation inherited by every future editor of a class [P21]. With the boundary at the carrier, the obligation is still there, but it has moved: the questions *which thread runs this*, *is this safely published*, *is there a happens-before edge* are answered in **register** and in the choice of queue, and whoever owns that file owns them. A developer who adds a field to **Tally** next year does not reopen them. One who adds a method that works on the object's own state does not reopen them. What would reopen them is the same thing that breaks the condition above — a reference handed out, a thread started — and that is visible in the diff, which a missing keyword never was. The obligation has gone from every editor of every registered class to the owner of one file — reduced, not removed, and reduced by roughly the ratio of registered classes to carriers.

## The leftovers

Stated together, so nobody has to reassemble them from the sections above.

**Cycles between positions deadlock.**

**call** from inside a position that is itself being **call**ed, directly or through a chain, stops both. The rule that prevents it is a convention. The carrier does not enforce it.

**Mutable arguments cross the boundary unchecked.**

Carrier-mediated access to the object is serialized; its inputs are not. Immutability at the seam is the caller's discipline, not the carrier's guarantee.

**The address is a string.**

No compile-time check, no IDE reach-through. The compiler does not check it; a typo surfaces at run time; the return type is whatever the call site claims; rename, find-usages, and autocomplete stop at the carrier. A check has moved off the compiler and onto the tests.

**One thread per position is one throughput.**

A position that does heavy work becomes a queue that grows. The carrier can hold a different policy — a pool per position, a shared pool with per-position ordering, a policy chosen per name — because **register** is where the policy lives and it is code you own. But a different policy changes the grades above: a pool per position readmits the second thread, and with it races and visibility, unless the position's class is written for it. The addresses do not change; the grades do.

None of these is hidden by the carrier. The first could be caught at run time — a carrier can notice that **call** is being made from one of its own position threads and refuse it — and **LePost** does not, because it is a proof of concept that walks the happy path. Swift's **Sendable** [8] and Pony's capabilities [9] address the second through compiler checks. Lemina's in-process binding chose not to enforce it, and the choice was made for a reason: enforcing it means changing what an argument is allowed to be, and not changing what a recipient or an argument is, is the whole reason a plain class can be registered as-is. The cost of the third — a string address with no compile-time check — is the trade the post that introduced **LePost** promised to return to [P23].

## Why not an actor framework

Every fix in the grades above is available from Akka, from Swift's **actor**, from any actor runtime. The difference is not in what is fixed. It is in how the boundary is set.

Those systems set it by asking the recipient to declare itself. Your class becomes an actor: it receives its messages through a declaration the runtime defines — a base class, a **receive** method, a behavior function — and it lives inside a runtime that decides when it runs. The boundary is real, and the object paid for it by becoming a different kind of object.

**The boundary — set without a declaration**

With Lemina, the recipient is reached through infrastructure, and the infrastructure is where **LePost** sets the boundary. The class stays a class, and the carrier is a file. This follows from the model it comes from: Lemina specifies the address and the act of posting to it, and deliberately leaves the carrier's delivery behavior open, so that the thread policy is a choice a binding makes [5][6]. The same addresses can carry another policy later; whether the grades survive it has to be checked again.

**The programming model — ordinary on both sides**

The programming model on both sides stays ordinary. The recipient is still a class with methods, written as a sequence of operations on its own state. A caller outside a position still writes **int sum = lePost.call("adder#add", a, b)** and uses the result in the next statement; the earlier post made the point that **call** is an imperative statement with the control flow unchanged [P23]. The one place the model does change is inside a position, where coordination becomes asynchronous: use **sendWithReplyTo**, so that waiting for an answer does not block the position's thread. What happens when that rule is broken is the first leftover above.

**Adoption — one object at a time**

Nor is it all or nothing. An object is lifted into Lemina by being registered, and only the objects that need the boundary have to be. The rest of the program keeps calling by reference, and a registered **Tally** can be the one object in a codebase that otherwise never sees an address. The condition from the grades applies per object: its callers switch to addresses and drop the reference; unrelated objects and their call sites stay as they are.

**The carrier — the team's**

**LePost** is about 170 lines the application owns, and the part that decides which thread runs your code is in them [P23]. An actor runtime lets a team configure its dispatchers, and sometimes supply one; it does not hand over the delivery path. Here the delivery path is the file. Building a runtime is not an afternoon's work; building this proof of concept was.

## Where the shape comes from

This shape has a lineage, and it is worth naming so the reader can see what is borrowed and what is not. The **Active Object** pattern, from 1996, puts a scheduler and a queue of method requests in front of an ordinary object so that the scheduler's thread, not the client's, runs the method [1]. The **actor model** goes further back, to Hewitt in 1973 [2]; Agha's 1986 formalization defines a unit of computation that has an address, a mail queue, and handles one message at a time [3]. Erlang is its best-known industrial form, and Armstrong's thesis is the argument for why isolated units that share nothing are the units you can build reliable systems from [4].

**LePost** takes one thing from both: the recipient handles one message at a time. It adds a policy neither dictates — one dedicated thread per unit, messages in arrival order. What it drops — the recipient's declaration and the framework — is the subject of the section above; what lets it drop both is the infrastructure underneath: the carrier already stands between every sender and every registered object, so the queue and the thread go there, and the recipient is not asked for anything [5][6].

## What the two posts add up to

The previous post made the diagnosis: Java's concurrency problems are structural because they come from where threads meet objects [P25]. This post followed it through two contributions of different kinds. Lemina addressing is infrastructure — a second way for a sender to reach an object, and one that passes through a carrier. **LePost** binds that infrastructure in-process and implements one policy on it — one position, one thread, with queues that establish visibility. The infrastructure made a place for the decision; the policy made it.

Under the confinement condition stated above, the result is two problems fixed, one fixed inside a position and moved between positions, one moved, and one reduced. Shared mutable data and blocking dependencies between positions remain possible, so those problems remain too. The scope of the fix follows the scope of the structural change.

The fix is structural because the problem was. That is the whole claim, and it is now made in full. The promised argument about the cost of string addressing is next.

---

## References

[1] R. Greg Lavender and Douglas C. Schmidt, "Active Object: An Object Behavioral Pattern for Concurrent Programming," in *Pattern Languages of Program Design 2*, Addison-Wesley, 1996. The scheduler-in-front-of-an-object shape **LePost** borrows its execution discipline from. <https://www.dre.vanderbilt.edu/~schmidt/PDF/Act-Obj.pdf>

[2] Carl Hewitt, Peter Bishop, and Richard Steiger, "A Universal Modular ACTOR Formalism for Artificial Intelligence," *IJCAI*, 1973. The origin of the actor model: addressed units that communicate by message. <https://www.ijcai.org/Proceedings/73/Papers/027B.pdf>

[3] Gul Agha, *Actors: A Model of Concurrent Computation in Distributed Systems*, MIT Press, 1986. The standard formalization: an actor has an address and a mail queue and handles one message at a time. It does not prescribe a thread per actor or arrival-order delivery; those are **LePost**'s policy, not the model's. <https://mitpress.mit.edu/9780262010924/actors/>

[4] Joe Armstrong, *Making Reliable Distributed Systems in the Presence of Software Errors*, PhD thesis, KTH, 2003. The case for isolated, share-nothing processes as the unit of reliability; the source of the copy-on-send discipline this post contrasts with. <https://erlang.org/download/armstrong_thesis_2003.pdf>

[5] Rex Young, "Addressing as an Overlay Without IDL: One Sender Surface from In-Process to Cross-Language," 2026. The addressing model **LePost** applies to in-process Java: a textual address, one-way delivery through a mediating carrier, and recipients that need no interface, base class, or generated stub. <https://doi.org/10.5281/zenodo.21232901>

[6] Rex Young, *Lemina Addressing Metamodel*, 2026. Leaves the carrier's delivery behavior open by design, which is why the thread policy is a binding's choice rather than the model's rule. <https://doi.org/10.5281/zenodo.20680627>

[7] Java Platform SE 21 API Documentation, "Memory Consistency Properties," **java.util.concurrent** package summary. Documents the happens-before edge from putting an element into a **BlockingQueue** to taking it out, and from the completion of a **Future** to the **get** that retrieves its result. <https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/package-summary.html>

[8] Swift Evolution SE-0302, "**Sendable** and **@Sendable** closures," 2021. The compiler-checked requirement on values that cross an actor boundary; **@unchecked Sendable** lets an author opt out of the check. <https://github.com/apple/swift-evolution/blob/main/proposals/0302-concurrent-value-and-concurrent-closures.md>

[9] Sylvan Clebsch, Sophia Drossopoulou, Sebastian Blessing, and Andy McNeil, "Deny Capabilities for Safe, Fast Actors," *AGERE!*, 2015. Reference capabilities: isolation of data crossing an actor boundary, as a type. <https://doi.org/10.1145/2824815.2824816>

## Earlier in this series

- [P21] [Exposed Concurrency: A Standing Obligation, Not Just a Hiring Requirement](https://lnkd.in/p/gzSsKrsG)
- [P23] [Take the Races Out of Your Java Objects, and Out of Your Head](https://lnkd.in/p/gQsHBWEq)
- [P25] [Java's Concurrency Problems Are Structural; So Is the Fix](https://lnkd.in/p/gn5s8vux)
