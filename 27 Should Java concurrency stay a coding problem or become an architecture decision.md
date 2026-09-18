**Should Java concurrency stay a coding problem, or become an architecture decision?**

*Originally published on LinkedIn: <https://lnkd.in/p/gt9SFt3J>*

Java, C++, C#, Kotlin, Scala, Python, Ruby, Objective-C, and pre-actor Swift all support OOP with **shared-memory threading**: a method call runs the callee on the caller's thread over one shared heap, so any thread that reaches a mutable object can run it. Keeping that safe rests on assumptions that **every future developer** inherits.

These four posts trace the maintenance burden, demonstrate an alternative, explain why it works, and examine its limits:

**1. Exposed Concurrency: A Standing Obligation, Not Just a Hiring Requirement** https://lnkd.in/p/gzSsKrsG

An ordinary change can invalidate an assumption the design quietly depends on. The change looks routine; the reasoning it reopens is specialized and often lives outside the changed lines. Memory-model knowledge stops being interview vocabulary and becomes an obligation to preserve across people and time.

**2. Take the Races Out of Your Java Objects, and Out of Your Head** https://lnkd.in/p/gQsHBWEq

A plain counter races when multiple threads increment it without coordination. Invoke it through a carrier instead: **tally.add(1)** becomes **lePost.call("tally#add", 1)**. The object is registered under a text name; the sender's thread stops at the carrier, and the thread bound to that name runs the code. With all access confined to that name, the race disappears, and the class stays unchanged. Which thread runs an object becomes **one decision, in one file**, in a proof of concept written in about 170 lines you own.

**3. Java's Concurrency Problems Are Structural; So Is the Fix** https://lnkd.in/p/gn5s8vux

Why does that work? Java's **object boundary** controls member access, not which thread may enter: there is no **execution boundary**. Races, visibility gaps, non-composing locks, and unenforced isolation follow. Virtual threads made threads cheap and left the model alone.

**4. Lemina Is an Architectural Fix for Java's Concurrency** https://lnkd.in/p/gSxvSmfa

Lemina is an addressing model: a sender invokes an object by text address through a **carrier between sender and recipient**. LePost, an in-process carrier, uses that placement for one execution policy: a thread per registered name. Under confinement, races and visibility gaps are resolved within the protected state, and **execution-policy reasoning is centralized in the carrier**. Application developers still have to preserve confinement.

The class needs no actor framework declaration: it is registered as-is, one object at a time; unregistered objects keep calling by reference. Still open: mutable data crossing the boundary, blocking cycles, per-object throughput, and string addresses without compiler checks.

These posts argue for making **execution policy an infrastructure decision**, so ordinary objects stay ordinary.

Threads still do the work. How much of their coordination should still fall on everyone who edits the application?
