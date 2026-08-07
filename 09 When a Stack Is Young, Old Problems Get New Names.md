# When a Stack Is Young, Old Problems Get New Names

Agentic AI keeps producing engineering terms.

Context engineering. Loop engineering. Graph engineering.

The names are useful. Their shapes are familiar.

Context engineering draws on state, memory, scope, retrieval, and model input. Loop engineering draws on control flow, retries, schedulers, supervisors, and stop conditions. Graph engineering draws on workflows, state machines, actors, and message passing.

In their current agent-engineering sense, the labels are new. The architecture underneath them is not.

The new part is that an LLM is now inside it.

Each concern becomes visible where deterministic contracts no longer cover the whole task.

Context can be assembled mechanically. Retrieval does this all day. But no general mechanism can certify that the facts, messages, and tool results selected for the model matched the user's intent.

Timeouts, token budgets, iteration counts, and success flags can stop a loop deterministically. But when continuation depends on whether the task is really done, the answer is good enough, or a human should be asked, the stop condition becomes semantic.

A graph can route on a fixed rule. But when the next node depends on which specialist, tool, escalation, or process path best fits a case, routing becomes judgment.

That is why these concerns remain exposed. An abstraction can hide the mechanics. It cannot eliminate the semantic decision.

Nobody thinks about the memory model when they write a web app. Everybody thinks about context management when they build an agent.

The dates make the point.

Take three examples.

Context engineering entered popular AI circulation in June 2025 [1]. By June 2026, loop engineering had entered the current conversation [2]. The graph-engineering wave surfaced in July 2026.

Days into that wave, LangChain published *3 Years of Graph Engineering with LangGraph* [3]. Its own article says the term had surfaced that weekend; the graph practice was already three years old. The architecture beneath both was older still.

The oldest agent-era usage here just turned one. The youngest current wave began in July 2026. The problems underneath are decades old.

That is not a criticism. Naming is part of how a field forms, and these names are doing real work.

When a stack is young, old problems get new names.

One of those stacks stopped having to explain itself. The other is still doing it in public.

---

## References

[1] Tobi Lütke, post on X, 18 June 2025; Andrej Karpathy, post on X, 25 June 2025. <https://x.com/tobi/status/1935533422589399127> · <https://x.com/karpathy/status/1937902205765607626>

[2] Addy Osmani, *Loop Engineering*, 7 June 2026. <https://addyosmani.com/blog/loop-engineering/>

[3] Sydney Runkle and Harrison Chase, *3 Years of Graph Engineering with LangGraph*, 22 July 2026. <https://www.langchain.com/blog/3-years-of-graph-engineering-with-langgraph>
