# LLMs, CPUs, and Brains All Have a Context Window. LLMs Are the Third Time We Face It.

*Originally published on LinkedIn: <https://lnkd.in/p/gRTeyzey>*

An earlier post in this series argued that the CPU stack and the LLM stack rest on different foundations — specification in one case, judgment in the other. But different foundations are not new to us. We have already developed two stacks — one around CPUs, one around human cognition — that rest on different foundations. What connects them is not the foundation. It is a constraint they share.

The constraint is straightforward: each system can only work with a bounded amount of information at a time. Everyone building with LLMs knows the term for it. What is less obvious is that CPUs and brains have their own version of it — and that people have been developing disciplines around this constraint for decades.

## The constraint (context window)

An LLM works within a context window. In 2024, 200K tokens was a frontier. By 2025, leading models accepted over 1M. The numbers grow, but the boundary is real. Information outside the context window does not exist for that call unless something external supplies it.

A CPU has a context window too — its registers. The Intel 8086, the processor that launched the x86 architecture in 1978, had 8 general-purpose registers. A modern x86-64 processor has 16. Everything else lives in memory hierarchies that require additional operations to reach. Only what fits in registers is in immediate view at any given step.

A human brain has a context window too — working memory. In 1956, George Miller described its capacity as roughly 7 items, plus or minus 2 — a finding that has been cited 46,810 times [1]. Nelson Cowan's later reconsideration, controlling for rehearsal and grouping, brought the estimate closer to 3 to 5 [2]. The exact number varies with the person, the material, and the task. But the boundary is real: a few meaningful chunks at a time is what human cognition has to work with.

Three systems, three different mechanisms, three very different scales. The same structural pattern: a bounded number of working units available for immediate processing. Nothing beyond that boundary is available unless something brings it in.

## Twice before

People have faced this constraint twice before — and both times, they built entire disciplines around it.

Around the CPU, people developed operating systems, compilers, programming languages, memory hierarchies, and the practices of software engineering. These did not emerge because CPUs are unlimited. They emerged because CPUs are bounded and the work people needed to do with them was not. Every layer of the CPU stack is, in some sense, a response to the distance between what a processor can hold in one step and what a useful system must accomplish.

Around the brain, people developed something parallel. Requirements, design, implementation, testing, deployment, maintenance — the phases of a development lifecycle distribute cognitive work across people, roles, artifacts, and time. No one person holds a complete system in mind. Organizations manage this through coordination: responsibility boundaries, shared artifacts, reviews, feedback loops. Jay Galbraith traced this directly — organizational structures emerge as responses to information-processing demands that exceed individual capacity [3]. Kevin Crowston analyzed software processes through the coordination mechanisms that hold distributed, interdependent work together [4].

What compilers and operating systems are to the CPU's context window, organizational structures and development practices are to the brain's. Both are stacks built around bounded immediate capacity.

Neither arrived fully formed. In August 1969, Edsger Dijkstra described experiments with structuring programs — early work on structured programming [5]. His concern was program size: how to compose large programs whose correctness could still be demonstrated. What stands out in that document is not the technique but the posture: he was investigating a problem he could describe but not yet fully resolve, developing ways of thinking while the discipline was still forming. The discipline was taking shape through the investigation.

## The third time

Others have already noticed that computing history has something to offer LLM development. Beren Millidge modeled the LLM as a natural-language processor, mapped context to RAM, and explored what abstractions might form around it [6]. The CoALA framework drew on cognitive science and computing history to map the design space for language agents [7]. These efforts draw on earlier abstractions and architectures while investigating what remains unresolved in LLM development.

This article puts the emphasis on how the earlier disciplines themselves took shape. Their history gives LLM builders a starting point for investigation: how do we organize work that exceeds what any one call can hold? What information must each call receive? What decisions must survive a handoff? How do we notice when something important has been left behind?

These questions become concrete in the systems we build. A task may fail because a later call no longer has a requirement established earlier. Investigating what was lost, how it was lost, and how to preserve it turns a context limit into a problem we can learn from. Over time, that work can give us practices for keeping a larger task coherent across many bounded calls.

LLMs are the third time we face a context window in a system we need to build around. The earlier stacks show how much can be accomplished when work is organized beyond the capacity of a single step or a single mind. The opportunity now is to develop a discipline in which no single call needs the whole task in view for the system to carry it through.

---

## References

[1] George A. Miller, "The Magical Number Seven, Plus or Minus Two: Some Limits on Our Capacity for Processing Information," *Psychological Review* 63(2), 1956. Described measurable limits on immediate memory and absolute judgment. The specific numbers are experimental observations, not fixed specifications. https://doi.org/10.1037/h0043158

[2] Nelson Cowan, "The Magical Number 4 in Short-Term Memory: A Reconsideration of Mental Storage Capacity," *Behavioral and Brain Sciences* 24(1), 2001. Reconsideration of Miller's estimates under controlled conditions; proposes roughly three to five chunks as the capacity of the focus of attention. https://doi.org/10.1017/S0140525X01003922

[3] Jay R. Galbraith, "Organization Design: An Information Processing View," *Interfaces* 4(3), 1974. Connects individual cognitive limits to organizational structure as a response to information-processing demands under uncertainty. https://doi.org/10.1287/inte.4.3.28

[4] Kevin Crowston, "A Coordination Theory Approach to Organizational Process Design," *Organization Science* 8(2), 1997. Analyzes a software change process through dependencies and coordination mechanisms that connect distributed work. https://doi.org/10.1287/orsc.8.2.157

[5] Edsger W. Dijkstra, "Structured Programming," EWD268, August 1969. Describes experiments and reflections on structuring programs, working at the boundary of available evidence while the discipline was still developing. https://www.cs.utexas.edu/~EWD/transcriptions/EWD02xx/EWD268.html

[6] Beren Millidge, "Scaffolded LLMs as Natural Language Computers," April 2023. Models an LLM as a processor and its context as RAM; explores possible abstractions by analogy with computing history. https://www.beren.io/2023-04-11-Scaffolded-LLMs-natural-language-computers/

[7] Theodore R. Sumers, Shunyu Yao, Karthik Narasimhan, and Thomas L. Griffiths, "Cognitive Architectures for Language Agents," 2023. Draws on cognitive science and computing history to propose a framework for language agents and identify research directions. https://arxiv.org/abs/2309.02427

## Earlier in this series

- [The CPU Stack Runs on Specification. The LLM Stack Runs on Judgment.](Post 10)
