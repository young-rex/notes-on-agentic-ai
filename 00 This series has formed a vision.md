## This series has formed a vision.

*Originally published on LinkedIn: <https://lnkd.in/p/gZR-S3KV>*

**1. Identify three processors, three stacks**

A CPU is the processor of the software stack; call it the CPU stack. Its **working context** is its registers; its **long-form context** is a process — a running program a human wrote.

An LLM is the processor of a new stack. Its working context is its context window. A goal from a human stretches its work past one call; its long-form context — the counterpart of a running program — has no settled form yet.

A person is the processor of the oldest stack. Their working context is the few subjects a mind holds at once; their long-form contexts nest — a role taken up for years, a vocation chosen for life — and at every level the person is **self-motivated**.

Not definitions — a similarity strong enough to set the three side by side. Then the differences show. Some work can be **specified**; the rest needs **judgment**. The CPU stack holds only the specifiable half. The human stack holds both and answers for both. The LLM stack holds judgment and cannot answer for it: it takes **duties, not accountability**. And two stacks are mature — seventy-five years for one, centuries for the other — while the third is three years old and mostly unbuilt.

**2. Add Lemina**

The Java concurrency posts look like a detour. They are the entrance. They bring in **Lemina**, an addressing model with two aspects. **Addressing:** a sender reaches a recipient by a text name, through a carrier. **Activation:** the machinery around a passive recipient wakes it, runs it, holds for it, speaks for it — the Java binding does it with a thread and a queue in front of a plain object, and the race is gone.

**3. Create a level ground**

Objects from the CPU stack, agents from the LLM stack, people from the human stack: with Lemina, each has an address of the same form, each reached by **one act** — post a message and walk away. Fourteen messages booked a dinner that way (Post 24). Lemina unifies **reaching** and **interaction**.

**4. Form a vision**

On level ground, the two established stacks stop being analogies for the LLM stack and become **parts to build it with**. Languages, paradigms, programming models, architecture from one. Roles, duties, processes, management from the other. Some will transfer **as-is**; some only as **inspiration**, because the LLM stack rests on judgment where the CPU stack rests on specification. **Build the new stack from the two we already know how to build.**

It is **a vision, not a method**. What drives the LLM stack, and what is its "program"? The three-tenant address space is fourteen messages, not a system. Pieces are missing; they stay missing here.

Twenty-seven posts are the outline; this is the shape.


---

# Notes on Agentic AI — The Posts

The outline behind the vision: twenty-seven posts, in the order they were written. Full text of every post: https://github.com/young-rex/notes-on-agentic-ai

## The map

| Step | Posts |
|---|---|
| 1A. Identify three processors, three stacks — the LLM stack and its boundary with code | 1, 2, 3, 7, 8, 9, 10, 11, 14, 15, 16 |
| 1B. — the human stack: duties, accountability, the enterprise | 4, 5, 6, 12, 13, 17, 18, 19, 20 |
| 1C. — the three side by side | 22 |
| 2. Add Lemina | 21, 23, 25, 26, 27 |
| 3. Create a level ground | 24 |
| 4. Form a vision | this post |

---

## 1. Write Software That Uses Agentic AI

https://lnkd.in/p/gmh8qr5J

Using AI to write software is one job; writing software that uses agentic AI is another, and harder. The root problem: code has no cheap, deterministic way to check that an agent's output satisfies the intent behind it.

## 2. Agents Break Open Software's Stable Relationship

https://lnkd.in/p/gVtniMsP

For decades, humans expressed intent in code and CPUs executed it. Agents add a participant with a different native speed and a different kind of processing, creating a speed boundary and a semantic boundary that engineering contracts do not yet cover.

## 3. Does Software Ever Need to Call an Agent?

https://lnkd.in/p/g822ukCZ

Code → Agent → Code is the one arrangement that pays both bills — speed and semantics — at once. A two-axis map says where it survives and why an enterprise should not put open-ended agent judgment in its fast lane by default.

## 4. AI Took Programming, Not Engineering. Will It Do the Same to Other Roles?

https://lnkd.in/p/gz73NR4J

A role is a bundle of duties. The role is the unit to decompose; duties are the unit to allocate between people and AI — software development simply made this visible first.

## 5. When AI Splits the Duties, Redraw the Business Process

https://lnkd.in/p/g9UUxGaM

Roles do not operate alone; a business process connects them. When duties inside the lanes move to AI, the flows between the lanes — handoffs, reviews, decisions, controls — must be redrawn too.

## 6. AI Takes Duties, Not Accountability

https://lnkd.in/p/g83-xYpn

For the first time, an organization has given real duties to a worker that cannot bear any accountability for them. The accountability does not split; it falls entirely on the nearest human.

## 7. We Built the World Around the CPU. We Are Doing It Again Around the LLM.

https://lnkd.in/p/gZ4QkT-Z

Put the LLM in the processor slot and look at the stack growing around it. Six rungs the CPU stack climbed over decades; the LLM stack is on the first one, barely.

## 8. Knowing Every Layer Is the Job. Building Every Layer Is Not.

https://lnkd.in/p/gReNQhZ2

The human–model–machine relationship is now a triangle with two young edges. The enterprise cannot wait for the stack to settle, and it should not build every missing layer itself: build where the workflow differentiates, wrap the unstable, wait where guarantees do not exist.

## 9. When a Stack Is Young, Old Problems Get New Names

https://lnkd.in/p/geCqmmfe

Context engineering, loop engineering, graph engineering: new labels on decades-old architecture, made visible again because an LLM now sits inside it and the semantic decision cannot be abstracted away.

## 10. The CPU Stack Runs on Specification. The LLM Stack Runs on Judgment.

https://lnkd.in/p/ga3apNfd

The CPU stack was built on behavior that could be fully specified; contracts, automation, abstraction, composition, and verification all followed from that one property. The LLM stack rests on judgment, so it will mature differently and with different mechanisms.

## 11. The Models Are Getting Better. Code Still Cannot Fully Check Their Judgment.

https://lnkd.in/p/gQqVvCsF

Function calling, structured outputs, reasoning models, instruction hierarchy: vendors can enforce what crosses the edge between the two stacks. They cannot tell code whether the judgment inside was right — verified shape, not verified judgment.

## 12. The Enterprise Chose Speed. Own the Burden.

https://lnkd.in/p/gsv6FUFr

At AI speed, the limit on per-output human review is arithmetic, not skill: it cannot keep up. Control moves to five levers — deployment, scope, monitoring, intervention, rollback — and the enterprise that chose speed must say who holds them.

## 13. Govern the AI, and Redesign the Work

https://lnkd.in/p/g-uNHNR7

After the manual worker and the knowledge worker, a third participant has entered the workforce — one that takes duties without accountability. Governance is necessary but not sufficient; the work itself must be redesigned to fit what AI actually is.

## 14. Agentic AI Moves Too Fast. Calibrate.

https://lnkd.in/p/gGShbrY4

A five-phase arc — model capability, action capability, agency, connectivity, harness engineering — makes a field that changes weekly legible, so a new term can be placed rather than chased.

## 15. Calibrate: Where Does the Capability Live?

https://lnkd.in/p/g7Es6PC3

For any agentic mechanism, ask whether it would survive swapping in an unmodified general-purpose model. Weights, harness, or hybrid — and the principle that follows: put judgment in the weights, put guarantees in the harness.

## 16. Calibrate: Choose a Model for Enterprise Refund Decisions

https://lnkd.in/p/gk_7yGX5

One enterprise feature, three mechanisms, one in each location, each demanding a different kind of evidence. Select what is learned, validate what is shared, enforce what must be guaranteed.

## 17. ENIAC Was Not Built for Business. Neither Was AI.

https://lnkd.in/p/gDV6yZsz

Business was a primary use of computing from the first machines, and it is the same with AI. But AI arrives into seventy-five years of accumulated systems, classified six different ways by six communities that never agreed.

## 18. Enterprise Systems Before AI: Academia, Vendors, Architecture, Process, Data, and Operations

https://lnkd.in/p/gPgiypEM

Six classifications of the same enterprise, side by side, with attention to where they contradict. The seams between them are where integration, reconciliation, and exception handling already live — and where AI is now reaching.

## 19. IT Hiring Is Broken Because Process Outranks Judgment

https://lnkd.in/p/gz6Khivv

The research says structure and judgment together predict hiring quality. Organizations invest in the half they can measure, until process no longer supports judgment but displaces it.

## 20. Some Work Can Be Spelled Out. The Rest Has to Be Trusted to Someone.

https://lnkd.in/p/gUrzSnVc

Every post in this series asks one question: which half of the work can be specified, which needs judgment, who holds each half, and can they answer for it. The hiring post is the control case — the same failure with no AI in it.

## 21. Exposed Concurrency: A Standing Obligation, Not Just a Hiring Requirement

https://lnkd.in/p/gzSsKrsG

Some code makes an ordinary change reopen concurrency reasoning that lives nowhere in the diff. That turns a specialized mental model into an obligation the organization must keep reproducing across every future maintainer.

## 22. LLMs, CPUs, and Brains All Have a Context Window. LLMs Are the Third Time We Face It.

https://lnkd.in/p/gRTeyzey

Registers, working memory, context window: three processors, three stacks, each bounded, each with a discipline built around the boundary. Twice before, people organized work beyond what one step or one mind could hold; this is the third time.

## 23. Take the Races Out of Your Java Objects, and Out of Your Head

https://lnkd.in/p/gQsHBWEq

Reach a plain Java object by a text name through a carrier instead of by reference, and the race disappears without touching the class. The carrier activates the object: which thread runs it becomes one decision, in one file, in about 170 lines you own.

## 24. In the Future, Objects, Agents, and People Will Live in the Same Program

https://lnkd.in/p/gfgHcrgm

Fourteen messages book a dinner: two objects, two agents, three people, one address form, one act of sending. The program unifies reaching; it does not unify trusting.

## 25. Java's Concurrency Problems Are Structural; So Is the Fix

https://lnkd.in/p/gn5s8vux

Java's object boundary decides who can see a name, not which thread may run the code. Races, visibility, non-composing locks, and unenforced isolation all follow from that one fact — and the fix is to move the boundary, not add a keyword.

## 26. Lemina Is an Architectural Fix for Java's Concurrency

https://lnkd.in/p/gSxvSmfa

Lemina's two aspects in one JVM: addressing, a second way to reach an object through a carrier; and activation, the execution policy the carrier sets. Five problems graded — fixed, moved, reduced — and the leftovers stated plainly.

## 27. Should Java Concurrency Stay a Coding Problem, or Become an Architecture Decision?

https://lnkd.in/p/gt9SFt3J

The four posts that bring Lemina into the series, on one page: the maintenance burden, the demonstration, the structural diagnosis, and the graded fix. The argument is for making execution policy an infrastructure decision so ordinary objects stay ordinary.
