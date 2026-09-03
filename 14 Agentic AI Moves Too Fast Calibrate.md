# Agentic AI Moves Too Fast. Calibrate.

*Originally published on LinkedIn: <https://lnkd.in/p/gGShbrY4>*

I follow the agentic AI movement closely. I still get confused — and some of what I learned with effort is already obsolete.

Terms shift meaning between weeks. A concept that was a research paper in March is a product feature in May and a deprecated approach by July. Everyone with a GitHub repository claims to have built a framework, a harness, an agent runtime. The cost is not only confusion despite attention. It is time spent learning things that did not last.

Two forces make this disorienting. First, the development cycle is measured in weeks, not months or years. Second, the number of players is so large that distinguishing infrastructure from application, settled from experimental, and your concern from someone else's becomes genuinely hard — even if you are paying attention.

I wrote earlier that when a stack is young, old problems get new names. That is one source of the confusion. I also wrote that knowing every layer is the job, but building every layer is not. That is the other: without orientation, a practitioner cannot tell which layers to track, which to build on, and which to leave alone.

Both principles point to the same practice: periodic self-calibration. Not passive following. Not chasing. Deliberate surveying with structure.

This post begins one such exercise. The research was assembled by AI — it can survey a fast-moving field and organize it by structure faster than I can. What I contributed is the direction: which questions to ask, which lenses to apply, and which distinctions matter for the work I care about. The assembly is the AI's duty. The judgment is mine.

The first question asks how the field arrived where it is.

The answer is a five-phase arc. The engineering boundary moved steadily outward — from the model itself, to the model plus tools, to the agent loop, to the agent operating in an ecosystem, and finally to the entire harness and execution environment surrounding the agent. The field evolved from engineering intelligence inside the model to engineering the system that allows intelligence to act.

The attached table maps each phase: period, dominant question, and key developments. These phases overlap — each names when a concern became dominant, not when it was invented. A reader who sees this shape can locate where a new term or product fits on the timeline — and stop treating everything as equally novel.

But a timeline is only half a map. It tells you when something mattered. It does not tell you where the capability actually lives — in the model's weights, in the runtime around it, or in both. That architectural question is the subject of the next post.

---

## Post image

| Phase | Period | Dominant question | Key developments |
|---|---|---|---|
| 1. Model capability | 2020–2022 | How do we make the model know and reason? | Scale, instruction tuning, chain-of-thought, RAG |
| 2. Action capability | 2022–2023 | How do we let reasoning invoke external capabilities? | ReAct, Toolformer, function calling, JSON mode |
| 3. Agency | 2023–2024 | How do we make the model pursue a goal across multiple decisions? | Agent loops, memory, planning, reflection |
| 4. Connectivity | 2024–2025 | How do agents operate across tools, environments, and other agents? | Computer use, MCP, multi-agent systems, A2A |
| 5. Harness engineering | 2025–2026 | How do we make agents operate reliably over long horizons? | Context engineering, harness–compute separation, managed agents |
