# Java's Concurrency Problems Are Structural; So Is the Fix

*Originally published on LinkedIn: <https://lnkd.in/p/gn5s8vux>*

An earlier post in this series showed a race disappear from a plain Java object without touching the object [P23]. The post before it argued that concurrency in Java is a standing obligation — one that lands on every engineer who later edits the class, whether or not they knew they were doing concurrency work [P21]. Neither post said *why* the race was there in the first place.

This post does. The claim is in the title. **Structural** here means: the problems come from the shape of the model — from where threads meet objects — and not from any line of code. A missing **synchronized** is where a race *shows up*. It is not where it *comes from*.

## One fact

Everything below follows from one fact about Java, and it is easy to state.

A method call runs the callee's code on the caller's thread. Every thread in the process shares one heap — the same objects, the same fields [1]. Put those two sentences together: any thread that can reach an object can run that object's methods, and there is nothing in the language that says otherwise.

That is the whole model. The object boundary — **class Tally { private int count; }** — decides *who can see the name*. It does not decide *which thread may run the code*. It is a namespace boundary, not a thread boundary. When one thread is running, the difference is invisible. When a second one arrives, that difference is the only thing that matters.

## The model has a name, and it is the mainstream

The model is **shared-memory multithreading with locks**: threads share memory, and where that sharing would go wrong, a lock makes them take turns. Java did not invent it. The **synchronized** keyword is a descendant of Hoare's *monitor* from 1974 — a construct that bundles data with the lock that guards it and the queue of threads waiting for it [2]. The engineering discipline for living inside the model was written down, for Java specifically, in *Java Concurrency in Practice* [3].

Nor is Java alone in it. Look at the object-oriented languages people write concurrent programs in:

- **C++** — **std::thread**, **std::mutex**, and since C++11 a memory model built after Java's and on the same happens-before foundation, though it leaves a data race undefined where Java gives it a meaning [4].
- **C#** — **Thread**, **lock**, **Monitor** [5]; **async**/**await** is a layer on top, not a replacement.
- **Kotlin** and **Scala** — inherit the JVM's threads and heap; coroutines and Akka are libraries over the same model.
- **Python** — the **threading** module and its locks. The global interpreter lock limited *parallelism*; it never made ordinary Python code race-free. The free-threaded build, optional since 3.13, removes it [6].
- **Ruby** — **Thread** and **Mutex** over one heap [7].
- **Objective-C** and **Swift** — threads and dispatch queues over one shared heap.

Every one of these languages has objects, and every one of them lets any thread call any method on any ordinary object. That is why every one of them has a memory model, or a race detector, or a book titled *Concurrency in X*, and why those books all describe the same short list of problems. It is not a coincidence. It is the model they share.

## Five problems, one cause

Here is the list. Each item is derived from the one fact, not added to it.

**Races.** Two threads enter the same object and interleave inside it. **count += n** is a read, an add, and a write; a second thread can slip between any two of them. The Java Language Specification defines this precisely as a *data race*: two conflicting accesses not ordered by the language's rules [1]. The **Tally** numbers in the earlier post — 64,000 expected, about 20,000 delivered — are what that definition looks like at run time [P23].

**Visibility.** This is the one most engineers do not know they do not know. A write performed by one thread is not guaranteed to be *seen* by another thread, ever, unless the two are connected by what the Java Memory Model calls a **happens-before** edge — a formal guarantee that what one thread did before some event is visible to another thread after a matching event [8]. Locks create such edges. So do **volatile** fields, starting and joining a thread, and the operations **java.util.concurrent** documents as creating one; the language specification lists the full set of language-level edges [1]. Where no such edge applies, cross-thread visibility is simply not guaranteed, and the compiler and CPU are free to reorder. In the specification's terms, an unsynchronized write and read of the same field are a data race just as two interleaved increments are. Engineers meet them differently. The first item above is about *interleaving*: two threads inside one operation. This one is about whether the other thread's write *exists* from where you stand — a single write, a single read, no interleaving at all, and still no guarantee the read sees it. A missing edge may show up in a test, or may not; it can pass on one machine and fail on another with a different JIT tier or CPU. What it never does is announce itself in the code.

**Locks do not compose.** Take two classes, each correctly locked in isolation. Combine them — move money from one account to another — and the combination is not correct: either it is not atomic, or it needs a new lock over both, and now the lock order between them can deadlock. The clearest statement of this is in the paper that introduced composable transactions to Haskell, whose second section is a catalogue of the ways lock-based components fail to combine [9]. The point for Java is that local correctness does not add up. Every new combination is a new judgment.

**Isolation is not enforced.** Nothing in the language stops a thread from reaching into an object. A class may be *written* to be confined to one thread, but the confinement is a convention, checked by nobody. The research literature knows what enforcement would take — *ownership types*, a type system that tracks which object owns which and can therefore reject a data race at compile time [10] — and the fact that the work had to be done shows what Java does not have. In Java, isolation is a comment.

**The obligation does not expire.** Because the four problems above live in the model and not in any particular line, every future edit to the class reopens all four. A developer who adds a field has changed the visibility question. One who adds a method that calls out has changed the lock-composition question. The earlier post in this series is about what that costs — who carries it, and for how long [P21]. Here it is enough to note where it comes from: it is the fact in section one, applied over time.

## What Java fixed, and what it left where it was

Java has changed a great deal since the model was set, and it is worth being precise about what changed.

**Thread cost was fixed.** A platform thread is an operating-system thread: expensive to create, expensive to keep. Virtual threads, delivered in Java 21, make threads cheap enough to run a million of them [11]. The design document behind them is explicit that this is the *only* thing that changed: "virtual threads are just threads" — same API, same semantics, same locks [12]. The JEP says it in its non-goals: "It is not a goal to change the basic concurrency model of Java" [11]. A race on a virtual thread is the same race.

**Task lifetime is being fixed.** A thread started in one method could outlive the method, leak, and lose its error. **StructuredTaskScope**, in preview since Java 21, binds the lifetime of a group of tasks to a block of code, so that the block cannot exit while a task it started is still running, and a failure in one task can cancel the others [13]. This is a real improvement, and it is about *when a task ends*. It says nothing about *which thread may enter an object*.

**The model was left where it was.** Cheaper threads, scoped lifetimes on the way — and still, any thread that can reach an object can run its methods. The five problems are untouched, and they were left untouched on purpose.

## Some object languages moved the boundary instead

A few object-oriented languages did something else. They did not add better locks. They changed what an object boundary means.

- **JavaScript** runs one thread per agent, and every job runs to completion before the next begins [14]. Two callbacks on the same agent never interleave, so there are no races between them — at the cost of no parallelism inside the agent. (Shared memory between agents exists, is opt-in, and brings the races back with it; the default is the run-to-completion rule.) The boundary here is the agent, not the object: every object inside it is protected at once, by the rule that nothing else runs.
- **Swift**, from version 5.5, has **actor** types: an object whose state is touched by one task at a time between suspension points, enforced by the compiler, which rejects a synchronous call to that state from outside — an outside caller must **await** it [15]. The actor may interleave two callers at an **await**, but never inside a synchronous stretch of code. Same language as the shared-memory Swift in the list above. The boundary moved for one kind of object.
- **Pony** is an object-oriented actor language whose type system, through *reference capabilities*, makes a data race a compile error [16]. That is the far end of the spectrum: isolation not as convention but as a type.

In each of these there is an **execution boundary**, around the agent in JavaScript and around the object in Swift and Pony, and it decides not only who may see a name but how the code behind it may be entered: by one thread, by one task at a time, by holders of one capability. The mechanism differs — an event loop, an **actor** keyword, a type — and none of them promises a dedicated thread; what they share is that the decision is made once, at the boundary, and not at every call site.

## The fix is where the boundary is

That is the argument. Java's concurrency problems are structural because the boundary is: an object boundary in Java says who can see a name and nothing about how the code may be entered. A keyword on a method does not move the boundary. It guards one path into the object, and leaves every other path, present and future, to be guarded by someone else.

A boundary is a decision. In the languages above, the language made it. It does not have to be the language. The *Active Object* pattern showed, in 1996, that a scheduler standing in front of an ordinary object can make the same decision from the outside — the object's implementation stays ordinary; the pattern adds a proxy and method requests in front of it, and the scheduler decides which thread runs it [17].

The earlier post in this series made that decision in one file: **LePost**, an in-process carrier for the Lemina addressing model, in front of which a plain Java class was reached by a text name instead of by reference, and its race went away [P23]. The next post says exactly what it fixed, and what it did not.

---

## References

[1] James Gosling et al., *The Java Language Specification, Java SE 21 Edition*, Chapter 17, "Threads and Locks," 2023. Defines threads as sharing one heap, defines *data race*, and specifies the happens-before rules the rest of this post relies on. <https://docs.oracle.com/javase/specs/jls/se21/html/jls-17.html>

[2] C. A. R. Hoare, "Monitors: An Operating System Structuring Concept," *Communications of the ACM* 17(10), 1974. The construct Java's **synchronized**, **wait**, and **notify** descend from; Java's signaling semantics differ from Hoare's. <https://doi.org/10.1145/355620.361161>

[3] Brian Goetz et al., *Java Concurrency in Practice*, Addison-Wesley, 2006. The standard engineering account of working inside the shared-memory model. ISBN 978-0-321-34960-6.

[4] Hans-J. Boehm and Sarita V. Adve, "Foundations of the C++ Concurrency Memory Model," *PLDI*, 2008. The C++11 memory model: built on the happens-before foundation Java's work established, with data races left undefined rather than given semantics. <https://doi.org/10.1145/1375581.1375591>

[5] ECMA-334, *C# Language Specification*, 7th edition, 2023, §13.13, "The lock statement." <https://ecma-international.org/publications-and-standards/standards/ecma-334/>

[6] Sam Gross, PEP 703, "Making the Global Interpreter Lock Optional in CPython," 2023. Documents that the GIL limited parallelism, not the model, and makes it optional in a separate build. <https://peps.python.org/pep-0703/>

[7] Ruby documentation, class **Thread** and class **Thread::Mutex**. Threads over one heap, and the lock a Ruby thread uses to take turns. <https://docs.ruby-lang.org/en/master/Thread.html>, <https://docs.ruby-lang.org/en/master/Thread/Mutex.html>

[8] Jeremy Manson, William Pugh, and Sarita V. Adve, "The Java Memory Model," *POPL*, 2005. Defines happens-before and the visibility guarantees — and their absence — that Java gives across threads. <https://doi.org/10.1145/1040305.1040336>

[9] Tim Harris, Simon Marlow, Simon Peyton Jones, and Maurice Herlihy, "Composable Memory Transactions," *PPoPP*, 2005. Section 2 is the canonical demonstration that correctly locked components do not combine into a correct whole. <https://doi.org/10.1145/1065944.1065952>

[10] Chandrasekhar Boyapati, Robert Lee, and Martin Rinard, "Ownership Types for Safe Programming: Preventing Data Races and Deadlocks," *OOPSLA*, 2002. What it takes to make isolation a compile-time guarantee in a Java-like language. <https://doi.org/10.1145/582419.582440>

[11] JEP 444, "Virtual Threads," OpenJDK, 2023. Delivered in Java 21; the Non-Goals section states that changing the concurrency model is not a goal. <https://openjdk.org/jeps/444>

[12] Ron Pressler, "State of Loom," Part 1, OpenJDK, May 2020. The design rationale for virtual threads: the problem was thread cost, not the thread programming model. <https://cr.openjdk.org/~rpressler/loom/loom/sol1_part1.html>

[13] JEP 505, "Structured Concurrency (Fifth Preview)," OpenJDK, 2025. **StructuredTaskScope**: task lifetimes bound to a lexical scope. A preview feature, not yet final. <https://openjdk.org/jeps/505>

[14] ECMA-262, *ECMAScript Language Specification*, §9.5, "Jobs and Host Operations to Enqueue Jobs." The run-to-completion job queue behind JavaScript's single-threaded execution within one agent. <https://tc39.es/ecma262/#sec-jobs>

[15] Swift Evolution SE-0306, "Actors," 2021. Actor-isolated state: one task at a time between suspension points, enforced by the compiler; actors are reentrant at **await**. <https://github.com/apple/swift-evolution/blob/main/proposals/0306-actors.md>

[16] Sylvan Clebsch, Sophia Drossopoulou, Sebastian Blessing, and Andy McNeil, "Deny Capabilities for Safe, Fast Actors," *AGERE!*, 2015. Pony's reference capabilities, which make data races a type error. <https://doi.org/10.1145/2824815.2824816>

[17] R. Greg Lavender and Douglas C. Schmidt, "Active Object: An Object Behavioral Pattern for Concurrent Programming," in *Pattern Languages of Program Design 2*, Addison-Wesley, 1996. A scheduler in front of an ordinary object decides which thread runs its methods. <https://www.dre.vanderbilt.edu/~schmidt/PDF/Act-Obj.pdf>

## Earlier in this series

- [P21] [Exposed Concurrency: A Standing Obligation, Not Just a Hiring Requirement](https://lnkd.in/p/gzSsKrsG)
- [P23] [Take the Races Out of Your Java Objects, and Out of Your Head](https://lnkd.in/p/gQsHBWEq)
