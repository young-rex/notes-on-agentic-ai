# Calibrate: Where Does the Capability Live?

*Originally published on LinkedIn: <https://lnkd.in/p/g7Es6PC3>*

The previous post mapped agentic AI as a five-phase arc — from model capability through action capability, agency, and connectivity, to harness engineering. That tells you when the dominant problem changed. It does not tell you where the capability actually lives.

That is a more useful question for anyone deciding what to build, what to adopt, and what to leave alone.

For any agentic mechanism, ask: **could this mechanism still exist if I swapped in an unmodified general-purpose LLM?**

The distinction matters because without it, effort lands on the wrong layer — building what the next model will absorb, or waiting for weights to deliver what only the runtime can guarantee.

If no — the behavior materially depends on what the model learned during training or post-training — the capability lives in the **weights**. A harness can expose tools and enforce stop conditions, but the model's judgment about which tool fits the situation and what arguments to pass is learned.

If yes — the mechanism can be implemented outside the model without changing its weights — the mechanism lives in the **Harness**.

If the model and runtime solve different parts of the same problem, the mechanism is **Hybrid**. Structured outputs are the canonical case: training teaches the model to understand a schema and populate it meaningfully; constrained decoding guarantees that emitted tokens conform syntactically. In practice, Hybrid is where you feel both sides working: the harness enforces a contract it can check, while the model makes a choice it cannot.

The classification is not permanent. A behavior may begin as a prompt technique and later become partially learned through post-training — the taxonomy describes where a mechanism lives in a given design, not an eternal property. Some mechanisms span two categories; the table marks both.

The most useful principle that emerges:

**Put judgment in the weights. Put guarantees in the harness.**

The attached table is a broad, representative mapping of major agentic mechanisms to their capability location and historical phase. It is a view rarely assembled in one place: not what an agent is made of, but where each capability actually sits.

One example: context engineering sits entirely in the Harness column. An enterprise investing in it is building infrastructure the next model will not absorb — worth the effort. Coding-agent post-training sits entirely in Weights. An enterprise building its own version is competing with the model vendors — worth questioning.

That map is broad. It is not yet directed. A different purpose — enterprise transformation, platform engineering, security — would highlight different mechanisms and draw different conclusions from the same table. That directed reading is where the next posts go.

---

## Post image

| Mechanism | Phase | Weights | Hybrid | Harness |
|---|---|:---:|:---:|:---:|
| Instruction following | 1 onward | ● | | |
| Chain-of-thought prompting | 1 | | ● | |
| RAG | 1 onward | | | ● |
| Self-consistency | 1 | | | ● |
| ReAct | 2–3 | | ● | |
| Tool-use training | 2–3 | ● | | |
| Function calling | 2 onward | | ● | |
| Structured outputs | 4 onward | | ● | |
| JSON mode | 2 onward | | ● | |
| Tool registry and routing | 2 onward | | | ● |
| Reflexion | 3 | | ● | |
| Agent loop | 3 onward | | | ● |
| Planning module | 3–4 | | ● | |
| External memory storage | 3 onward | | | ● |
| Agentic memory / notes | 3 onward | | ● | |
| Checkpointing | 3 onward | | | ● |
| Retry / backoff | 3 onward | | | ● |
| Adaptive error recovery | 3 onward | ● | ● | |
| Computer use | 4–5 | | ● | |
| Computer-use RL | 5 | ● | | |
| MCP | 4 onward | | | ● |
| Multi-agent orchestration | 4 onward | | | ● |
| Subagents | 4 onward | | | ● |
| Handoffs | 4 onward | | ● | ● |
| A2A | 4 onward | | | ● |
| Agentic workflow | 4–5 | | | ● |
| Coding-agent post-training | 5 | ● | | |
| Agentic RL | 5 | ● | | |
| Context engineering | 5 | | | ● |
| Context compaction | 5 | | ● | |
| Agent Skills | 5 | | ● | ● |
| Sandbox | 5 | | | ● |
| Permissions | 5 | | | ● |
| Human approval gates | 5 | | | ● |
| Tracing and observability | 5 | | | ● |
| Evaluation harness | 5 | | | ● |
| Termination logic | 5 | | | ● |
| Long-horizon execution | 5 | | ● | |
| Harness–compute separation | 5 | | | ● |
| Harness engineering | 5 | | | ● |
