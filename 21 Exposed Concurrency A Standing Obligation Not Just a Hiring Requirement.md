# Exposed Concurrency: A Standing Obligation, Not Just a Hiring Requirement

*Originally published on LinkedIn: <https://lnkd.in/p/gzSsKrsG>*

I read two job postings from the same employer. Together they paired a parenthetical mention of the Java memory model and concurrency performance with a must-have for highly concurrent, data-intensive software involving throughput, backpressure, and batching.

From outside I could not tell how broadly that depth was expected. The postings said "highly concurrent," which is the phrase everyone reaches for. It is also the phrase that hides the question I care about.

## What "highly concurrent" does not tell you

We describe concurrency by volume. A little of it, or a lot of it. Highly concurrent. Those words measure runtime: how many threads, how much contention, how much throughput a system sustains under load. There is no common phrase for the other measurement, which is how much concurrency reasoning the code demands once the first version ships, from every person who changes it afterward. The two are not the same, and they come apart in a diff.

A developer adds a field to an order object. The order is built on a request thread and handed to a worker through a `BlockingQueue`. Everything written to that order before `put` is visible to the worker that takes it, so the original design published the order safely.

The new field is a discount code, and its value only becomes available after the handoff, so that is where the assignment goes: after `put`. That write falls outside the queue's guarantee, so the worker may miss it entirely and see the field's default.

Visibility is not the whole of it either. Even if the field were `volatile`, the worker could take the order and finish with it before the write happens at all. The repair is a protocol change, not a keyword: assign before the handoff, or send the discount as its own message. One small diff, two kinds of reasoning.

Nobody in that story did concurrency work. Someone added a field.

I use "exposed concurrency" for code with this property: correctness depends on a programmer reasoning explicitly about visibility, ordering, ownership, and interleaving, and ordinary changes can put that reasoning back on the table. Not concurrency that is merely present, and not concurrency that is hard once. Concurrency whose reasoning is exposed to the next diff.

An architecture with that property asks something that outlives the people who chose it. The requirement does not end at launch. It is inherited by every future maintainer, reviewer, and on-call engineer whose work crosses those boundaries.

Deep concurrency expertise is not the question. It is genuinely valuable, and some systems cannot be built without it. The question is whether an application's architecture should require it continuously.

That returns me to the postings. Either the memory model was vocabulary, something a candidate should be able to discuss in an interview, or it was operational knowledge, something maintainers use in the course of ordinary work and therefore have to keep sharp. The first is a hiring requirement. The second is a standing obligation, and the two commit an employer to very different futures.

## A rough scale, not a standard

Some concurrency work is covered by a documented guarantee, so the person making a change can rely on the library or the framework instead of reasoning it out. The rest sits at application level, where whoever changes the code next has to work out ownership, ordering, and interleaving for themselves. The scale below is a rough way to locate that boundary. I found no broadly accepted industry or academic standard for it, and Appendix A says why, so the levels are my own.

The important thing about the levels is that you cannot read one off the API. Choosing a thread-safe class does not set the level. What sets it is the rule the code has to keep true, and whether a single call is enough to keep it. The numbers do not measure how much concurrency runs, and they do not rank programmers. They label where the correctness argument for a change mainly lives: in one library operation, across operations, in an ownership boundary, or in the memory model. The cases are not exclusive, and an L3 design can still contain L4 boundaries.

### L1, correct by delegation.

`cache.put(key, value)` on a `ConcurrentHashMap`, where no invariant spans more than that one operation. The documented guarantees of the call, its atomicity and the visibility that comes with it, are the whole argument.

### L2, where delegation stops.

`if (!cache.containsKey(key)) cache.put(key, value)`, on the same map, where the rule is that a key is installed once and never overwritten. Each call is atomic and the pair is not, so two threads can both pass the check and the second overwrites the first. Same API, different level, because the invariant now spans two operations.

### L3, reduces exposed coordination by design.

`cacheOwner.execute(() -> { if (!cache.containsKey(key)) cache.put(key, value); })`, where `cacheOwner` is a single-threaded executor and every read and write of the cache happens inside one of its tasks. Its tasks run sequentially, so nothing can interleave between the check and the put. The check-then-act from L2 is now safe without a lock, and the map no longer has to be a concurrent one. The same family includes confinement, immutability, single-writer ownership, bounded queues, and explicit asynchronous boundaries. Note that a plain `Executor` does not promise sequential task execution, so the design rests on choosing an implementation that does.

### L4, rests on the memory model.

Reasoning a change through when no higher-level design guarantee covers all of it, so the argument drops to memory-model guarantees: `volatile`, final-field semantics, safe publication, or the ordering a `BlockingQueue` establishes between what the producer writes before enqueuing an element and what the consumer does after taking that element. The opening scenario is L4 because the new write landed outside that ordering.

They are not an accepted taxonomy. They are a way to talk about a single change and what it asks of the person making it.

Worth being explicit about L3, because it is easy to misread: L3 is good design. Confinement and single-writer ownership exist precisely to reduce how much reasoning the next person needs. Nothing here argues against them. The point is that such designs rest on boundaries an ordinary change can invalidate, and when that happens, someone who was not there for the original design has to reconstruct its reasoning.

Here is the second scenario, which is L3 rather than L4.

A component confines a mutable cache to a single-threaded executor. A developer adds an administrative endpoint and updates the cache directly from the request thread, because that is the thread the request arrives on. The diff introduces no concurrency primitive at all. It creates a second writer and invalidates the ownership boundary that let the design avoid locking.

Neither this change nor the added field was presented as concurrency work. Both invalidated an existing concurrency assumption.

Three things are in play from here, and the argument depends on keeping them apart. Exposed concurrency is a property of the code. The reasoning it demands is a mental model, and a mental model lives in a person. Carrying that model from each person to the next is a standing obligation, and that one lands on the organization. The first claim below is the one that has to hold. The other two describe what can follow if it does.

## Claim 1: Ordinary changes reopen concurrency reasoning

Where correctness rests on visibility, ordering, ownership, and interleaving, the mental model is not consulted once at design time. It is reconstructed, partially and under time pressure, by whoever makes a change that touches one of those relationships. Both scenarios above are routine product work.

The questions such a change can reopen look like this:

1. Is this new field safely published to the threads that will read it?
2. Is there a happens-before edge that makes this write visible to that read?
3. Is this compound action atomic, or is it check-then-act?
4. Does the invariant now span two structures, guarded by neither?
5. Does this call path acquire locks in a new order?
6. Is a lock held across a call into code I do not control?
7. Does this leak a confined reference across a thread boundary?
8. Does this introduce a second writer to single-writer state?

The first two are governed by the Java memory model and the memory-consistency guarantees documented for `java.util.concurrent`. The rest I assembled from practice, and much of it will be familiar from the standard concurrency texts, but it is not a published taxonomy. Its value is that any Java developer can check it against code they already own.

What makes these failures especially difficult is where the answers live. They are often not in the changed lines. The publication edge is in a queue several files away, the owning thread is a convention nobody wrote down, the asynchronous boundary is in a class the diff does not touch. `javac` does not prove these application-wide properties. Ordinary tests may never hit the offending schedule, and optional analysis tools cover different subsets of the list.

## Claim 2: The mental model has to survive team turnover

If ordinary changes keep reopening that reasoning, the model has to be available whenever they happen. That means surviving onboarding, handoffs, departures, maintenance, review, and incident response. Each of those is a transfer, and each transfer risks loss or distortion.

The research I can point to here supports a mechanism rather than a measurement, and it studies groups rather than software teams. Communication structure, roles, and routines can condition how much disruption turnover causes. Whether a newcomer picks up specialized knowledge depends partly on how the group is organized to transfer it, not only on that person's ability. That cuts both ways, which is the point: structure can reduce the risk as well as create it. Specialized knowledge held informally becomes a continuing dependency, one an organization has to actively reproduce rather than assume.

## Claim 3: The architecture shapes hiring and training

A capability an organization has to keep reproducing does not stay inside the codebase. Over time a team accumulates more than code: platform-specific skills, hiring criteria written around them, training choices, review practices, and habits of thought.

This is the narrowest of the three claims and I want to keep it narrow. What the research provides evidence for is that technology-aligned skills can be valuable, transferable, and worth cultivating deliberately. It does not establish Java or memory-model lock-in. My inference is that when architecture, hiring, training, and review routines all align around specialized expertise, changing direction may mean rebuilding part of that human system and not only the code.

## The three claims together

Put them together and the staffing consequence is not just a one-time hiring problem. It is a standing one. An organization in this position has to keep reproducing a specialized capability across successive maintainers, reviewers, and on-call engineers, not because concurrency is hard, but because the architecture has spread the obligation to reason about it across ordinary product work.

Appendix B summarizes the evidence behind these claims and its limits.

## What this argument does not claim

It is not an argument against advanced concurrency. Direct shared-memory coordination can keep coordination overhead low and support demanding throughput and tail-latency targets. Some low-level components genuinely justify maintaining memory-model expertise, and the engineers who have it are doing necessary work.

It is also not a claim that another architecture eliminates coordination failures. Nothing does. The narrower question is whether architecture can move part of the reasoning burden off the ordinary change.

## The argument, compressed

> Exposed concurrency is what an architecture produces when it spreads visibility, ordering, ownership, and interleaving obligations across ordinary application changes. It turns a specialized mental model into a recurring organizational dependency. Architecture can change how much of that model each programmer must carry.

That last sentence claims possibility, not superiority. Judging what a different architecture actually costs takes a comparison this article does not make. The next post introduces a different architecture.

## Appendix A — Why I found no standard to borrow

I went looking for an existing scale before writing my own. The research I found classifies bugs [6] and specifies semantics [1]. Each is rigorous about what it measures, and neither offers a scale for where an ordinary change's correctness argument lives. The industry side offered no more, since engineering ladders describe a programmer's scope and autonomy, which is a different object.

So the levels above are a synthesis. Treat them as a device for having a conversation, not a rubric for grading anyone.

## Appendix B — Evidence behind this post

The diagnosis here is an argument, not a research finding. Its chain runs: Java concurrency designs can leave L3 ownership and coordination boundaries, or L4 memory-model obligations, exposed to ordinary changes → those changes can reopen the associated reasoning → carrying that specialized mental model across people and time creates an organizational dependency → the requirement shapes hiring and training.

The first two links are argument, and the change scenarios carry them. The last two have supporting evidence about mechanisms observed elsewhere. **None of these papers measured Java, and none compared architectures.**

**Obligations.** The first two of the eight questions are governed by the Java memory model [1] and the memory-consistency guarantees documented for `java.util.concurrent`. The remaining six I assembled from practice, and they are familiar from the standard concurrency texts rather than drawn from the cited papers. The list is a prompt for review, not a finding.

**Continuity across people and time.** Argote, Aven and Kush [2] ran a laboratory experiment with 109 four-person groups on a visual programming task, manipulating communication network and turnover: fully connected groups performed better with stable membership, while centralized groups performed better under turnover. Rao and Argote [3] found, with three-person groups on a production task, that knowledge embedded in roles and routines conditioned how much turnover disrupted performance. *Limits:* laboratory groups, short sessions, no organizational context; [2]'s authors note a task-order confound. These show structure can condition turnover disruption, not that embedded knowledge always survives. The transfer to a Java codebase is my inference.

**Hiring and training.** Ge, Huang and Kankanhalli [4] analyzed over a million IT workers' public professional profiles against 368 US publicly listed software firms, 2002–2012, finding that hiring platform-skilled workers from firms high in platform human capital is associated with higher recipient-firm performance. Li, Tan and Yang [5] surveyed experienced IT professionals and found perceived firm-specificity and learning-related scale associated with investment in internal open-source human capital. *Limits:* both are observational, so association is not causation here. [4] studies platform knowledge *moving between firms* and argues for its wider applicability, making it evidence of value and transferability rather than of lock-in. [5] supports deliberate investment in technology-aligned expertise, not objective non-portability, and neither is about Java.

## References

1. Manson, Pugh & Adve. *The Java Memory Model.* POPL 2005. [DOI](https://doi.org/10.1145/1040305.1040336)
2. Argote, Aven & Kush. *The Effects of Communication Networks and Turnover on Transactive Memory and Group Performance.* Organization Science 29(2), 191–206, 2018. [DOI](https://doi.org/10.1287/orsc.2017.1176)
3. Rao & Argote. *Organizational learning and forgetting: The effects of turnover and structure.* European Management Review 3(2), 77–85, 2006. [DOI](https://doi.org/10.1057/palgrave.emr.1500057)
4. Ge, Huang & Kankanhalli. *Platform skills and the value of new hires in the software industry.* Research Policy 49(1), 103864, 2020. [DOI](https://doi.org/10.1016/j.respol.2019.103864)
5. Li, Tan & Yang. *OSS Adoption: Organizational Investment in Internal Human Capital.* Journal of Computer Information Systems 54(1), 42–52, 2013. [DOI](https://doi.org/10.1080/08874417.2013.11645670)
6. Lu, Park, Seo & Zhou. *Learning from Mistakes: A Comprehensive Study on Real World Concurrency Bug Characteristics.* ASPLOS 2008. [PDF](https://www.cs.columbia.edu/~junfeng/08fa-e6998/sched/readings/concurrency-bugs.pdf)
