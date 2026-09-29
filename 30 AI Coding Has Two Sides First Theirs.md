# AI Coding Has Two Sides. First, Theirs.

*Originally published on LinkedIn: <https://lnkd.in/p/gyg_buW9>*

## Someone builds the capability we use

Model labs and agent builders develop the coding capability we use: Anthropic with Claude Code, OpenAI with Codex, and the few other companies that train their own coding models. We apply it to our business software. Understanding their work helps us focus on the work we need to own. It is an old division: software engineers have long built on programming languages and operating systems made by others.

In this article, they are the model labs and agent builders, and we are business-app developers. On Post 28's map, they work on agentic AI; we work with it.

Post 29 said better models arrive on their own. For us, they do, because people on the other side build them.

This article covers their side: where the capability lives and how they built it. The next covers ours.

## Where the capability lives

Post 15 asked where a capability lives: in the weights (what the model learned during training) or in the harness (the software around the model). Its principle: **put judgment in the weights; put guarantees in the harness.**

In AI coding, both are built on their side, and the boundary between them moves. For four years, they have kept moving judgment from the harness into the weights.

My reading: AI coding has advanced by automating more of the attempt–feedback–repair cycle. Each capability below takes over part of it: thinking through the attempt, reading feedback from tools, repairing failures, and keeping the cycle going past one context window.

## What they trained into the models

**Thinking before acting (2022–2026).** Chain-of-thought prompting asked a model to write out its reasoning before answering [1]. Reasoning models then trained that behavior with reinforcement learning, first at OpenAI and then in the open with DeepSeek-R1 [2][3]. The latest models decide for themselves how much to think [4].

**Reasoning between tool calls (2022–2025).** ReAct prompted models to interleave reasoning with actions [5]. Models are now trained to do it; Claude 4 models "alternate between reasoning and tool use" [6].

**Fixing its own failures (2023–2025).** Reflexion had an agent reflect in words on failed attempts and try again, keeping the lessons in memory rather than in the weights [7]. Reinforcement learning on test results moves that loop into training: attempt a task, run the tests, reward what passes, update the model. OpenAI trained codex-1 this way on real-world coding tasks, so that it runs tests until they pass [8].

**Working past one context window (2023–2026).** Long tasks outgrow a single context window. The first fix lived in the harness: summarize older turns and carry the summary forward [9]. Now models are trained to do it. OpenAI trained GPT-5.1-Codex-Max to work across context windows through compaction [10]. Cursor applies each task's final reward to the summaries its model wrote along the way, so summaries that lose critical information are trained out [11].

## How they train it, and why it stays their side

Reinforcement learning on code needs tasks for the model to practice on, each with a runnable environment and an automatic way to judge the result.

**Practice tasks are manufactured.** Real issue-and-fix pairs are limited, so tasks are now synthesized, for example by introducing bugs that break existing tests in real codebases. One research project built 50,000 such tasks from 128 repositories [12]. Qwen ran 20,000 training environments in parallel [13], and its 2026 report describes about 850,000 synthesized tasks [14].

**Real use becomes training.** Cursor turns billions of tokens from real sessions into reward signals. A cycle takes about five hours, so an improved model can ship several times a day [15].

Our own use can feed that loop. On individual plans, the major coding agents may train on our sessions, including our code: Claude Code if we allow it [16], Codex unless we opt out [17], GitHub Copilot by default since April 2026 [18], and Cursor when Privacy Mode is off [19]. Business and enterprise plans generally exclude training by default.

The methods are public; most of the details above come from Qwen, Cursor, and academic groups. Building and running the training machinery takes substantial investment, and they have made it. Post 15 drew the practical conclusion: an enterprise building its own coding-agent post-training "is competing with the model vendors — worth questioning."

## What they built into the agents

They built the agents too. Part of what makes an agent usable lives in the harness, because code can guarantee or supply it:

- **An agent loop in our terminal.** The agent reads the repository, edits, runs commands, and checks its work. Research showed that an interface designed for the model improves results by itself [20]; Claude Code and Codex brought that loop to developers [21][22].
- **Working without asking at every step.** Sandboxes limit which files and servers an agent can reach, so it needs fewer permission prompts. Anthropic reports 84% fewer in its internal use [23].
- **Our context and tools.** Memory files such as AGENTS.md give the agent a project's instructions [24]; the Model Context Protocol connects it to our tools and data [25].
- **Parallel work.** Subagents grew into agent teams that "work in parallel as a team and coordinate autonomously" [4][26].

Other mechanisms are shared between the model and the harness. In function calling, the model chooses the tool and the harness runs the call [27]. With Agent Skills, the harness loads packaged instructions and the model decides when they apply [28]. With compaction as a platform feature, the platform decides when to summarize and the model does the summarizing [4].

We fill some of these in: we write the memory files, connect the tools, and set the permissions. But they built the parts.

## Where the line runs

The attempt–feedback–repair cycle runs in two loops, and they are easy to confuse:

- **Training.** They run it. It changes the model's weights, and so its future behavior.
- **Development.** We run it. It changes context, files, and checks; the weights stay fixed.

They own the training loop, where the model learns. We work in the development loop, where the agent applies that capability to our software: reading our code, making changes, running checks, and fixing failures. Whether our sessions feed theirs is decided on our side, by the plan we buy and the settings we keep.

Unlike a compiler, a model runs on judgment, not specification (Post 10). Its training rewards passing the checks in practice tasks; it does not see our unwritten requirements, or how our software has to live for years. That gap may narrow as training and verification improve. The line between their side and ours does not move with it: deciding what our software must do, and answering when it runs, stay with us.

Our side is not what is left over. It is work only we can do:

- **Requirements.** Only we know what the business needs. Post 28 called knowledge of the business the part no vendor can ship.
- **Context.** Our systems, data, and conventions live with us. The agent knows only what we give it.
- **Acceptance criteria.** Training taught the model to pass checks. Which checks define "done" for our software is our call.
- **Operating outcomes.** The model cannot answer for results (Post 8). When the software runs, someone on our side must.

We do not need to build their side. We need to understand it, so we can focus on ours. How we handle each of these responsibilities is the next article.

---

## Post image

**What they built into models and coding agents**

| Lives in | Technique | Started as | Milestones |
|---|---|---|---|
| Weights | Trained reasoning | Chain-of-thought prompting | o1 (2024), DeepSeek-R1 (2025), adaptive thinking (2026) |
| Weights | Reasoning between tool calls | ReAct prompting | Claude 4 (2025) |
| Weights | RL on test results | Self-critique loops | codex-1 (2025) |
| Weights | Trained compaction | Harness summaries | GPT-5.1-Codex-Max (2025), Cursor self-summarization (2026) |
| Weights | RL in real repositories | | codex-1; Qwen3-Coder, 20,000 parallel environments (2025) |
| Weights | Synthesized practice tasks | | SWE-smith, 50,000 (2025); Qwen3-Coder-Next, ~850,000 (2026) |
| Weights | Real-time RL from production use | | Cursor (2026) |
| Hybrid | Function calling | | 2023 onward |
| Hybrid | Agent Skills | | 2025 |
| Hybrid | Compaction as a platform feature | | Claude Opus 4.6 (2026) |
| Harness | Agent loop with tools designed for the model | | SWE-agent (2024); Claude Code, Codex CLI (2025) |
| Harness | Sandboxes and permissions | | Claude Code sandboxing (2025) |
| Harness | Memory files | | AGENTS.md (2025) |
| Harness | MCP | | 2024 onward |
| Harness | Subagents, then agent teams | | 2025, then 2026 |

**Built on their side. Used on ours.**

Put judgment in the weights; put guarantees in the harness. (Post 15)

## References

[1] Jason Wei et al., *Chain-of-Thought Prompting Elicits Reasoning in Large Language Models*, arXiv:2201.11903, January 2022. <https://arxiv.org/abs/2201.11903>

[2] OpenAI, *Learning to reason with LLMs*, September 12, 2024. <https://openai.com/index/learning-to-reason-with-llms/>

[3] DeepSeek-AI, *DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning*, arXiv:2501.12948, January 2025. <https://arxiv.org/abs/2501.12948>. Its rule-based rewards for code used predefined test cases.

[4] Anthropic, *Introducing Claude Opus 4.6*, February 5, 2026. <https://www.anthropic.com/news/claude-opus-4-6>. Adaptive thinking, four effort levels, context compaction, and agent teams.

[5] Shunyu Yao et al., *ReAct: Synergizing Reasoning and Acting in Language Models*, arXiv:2210.03629, October 2022. <https://arxiv.org/abs/2210.03629>

[6] Anthropic, *Introducing Claude 4*, May 22, 2025. <https://www.anthropic.com/news/claude-4>

[7] Noah Shinn et al., *Reflexion: Language Agents with Verbal Reinforcement Learning*, arXiv:2303.11366, March 2023. <https://arxiv.org/abs/2303.11366>

[8] OpenAI, *Introducing Codex*, May 16, 2025. <https://openai.com/index/introducing-codex/>. See also OpenAI, *Addendum to o3 and o4-mini system card: Codex*, the same day.

[9] Charles Packer, Sarah Wooders, Kevin Lin, et al., *MemGPT: Towards LLMs as Operating Systems*, arXiv:2310.08560, October 2023. <https://arxiv.org/abs/2310.08560>. When the context fills, it evicts older messages and keeps a recursive summary of them.

[10] OpenAI, *Building more with GPT-5.1-Codex-Max*, November 19, 2025. <https://openai.com/index/gpt-5-1-codex-max/>. Described as the first model natively trained to operate across multiple context windows through compaction.

[11] Cursor, *Training Composer for longer horizons*, March 17, 2026. <https://cursor.com/blog/self-summarization>. On CursorBench, Cursor's internal benchmark, self-summarization cut the error from compaction by 50% while using one-fifth of the tokens.

[12] John Yang, Kilian Lieret, Carlos E. Jimenez, et al., *SWE-smith: Scaling Data for Software Engineering Agents*, arXiv:2504.21798, April 30, 2025. <https://arxiv.org/abs/2504.21798>

[13] Qwen Team, *Qwen3-Coder: Agentic Coding in the World*, July 22, 2025. <https://qwenlm.github.io/blog/qwen3-coder/>

[14] Ruisheng Cao et al., *Qwen3-Coder-Next Technical Report*, arXiv:2603.00729, February 28, 2026. <https://arxiv.org/abs/2603.00729>. The tasks feed a pipeline of continued pretraining, supervised fine-tuning, specialized expert models, and distillation into one model.

[15] Cursor, *Improving Composer through real-time RL*, March 26, 2026. <https://cursor.com/blog/real-time-rl-for-composer>. In Cursor's own A/B tests, agent edits that persisted in the codebase rose 2.28%, and dissatisfied follow-up messages fell 3.13%.

[16] Anthropic, *Updates to Consumer Terms and Privacy Policy*, August 28, 2025. <https://www.anthropic.com/news/updates-to-our-consumer-terms>. Applies to Claude Free, Pro, and Max, including Claude Code used from those accounts; excludes Claude for Work, Claude for Government, Claude for Education, and API use. Data is kept for five years if the user allows training.

[17] OpenAI Help Center, *How your data is used to improve model performance*, accessed September 2026. <https://help.openai.com/en/articles/5722486>. Covers OpenAI services for individuals, including ChatGPT and Codex.

[18] GitHub, *Updates to GitHub Copilot interaction data usage policy*, March 25, 2026. <https://github.blog/news-insights/company-news/updates-to-github-copilot-interaction-data-usage-policy/>. Effective April 24, 2026; Copilot Business, Copilot Enterprise, and enterprise-owned repositories are excluded.

[19] Cursor, *Data Use & Privacy Overview*, updated September 3, 2026. <https://cursor.com/data-use>. With Privacy Mode on, customer data is not used for training.

[20] John Yang, Carlos E. Jimenez, et al., *SWE-agent: Agent-Computer Interfaces Enable Automated Software Engineering*, arXiv:2405.15793, May 2024. <https://arxiv.org/abs/2405.15793>

[21] Anthropic, *Claude 3.7 Sonnet and Claude Code*, February 24, 2025. <https://www.anthropic.com/news/claude-3-7-sonnet>

[22] OpenAI, *Codex CLI*, open-source repository, released April 16, 2025. <https://github.com/openai/codex>

[23] Anthropic, *Beyond permission prompts: making Claude Code more secure and autonomous*, October 20, 2025. <https://www.anthropic.com/engineering/claude-code-sandboxing>

[24] *AGENTS.md*, accessed September 2026. <https://agents.md>. Used by more than 60,000 open-source projects; stewarded by the Agentic AI Foundation under the Linux Foundation.

[25] Anthropic, *Introducing the Model Context Protocol*, November 25, 2024. <https://www.anthropic.com/news/model-context-protocol>

[26] Claude Code documentation, *Orchestrate teams of Claude Code sessions*, accessed September 2026. <https://code.claude.com/docs/en/agent-teams>

[27] OpenAI, *Function calling and other API updates*, June 13, 2023. <https://openai.com/index/function-calling-and-other-api-updates/>

[28] Anthropic, *Introducing Agent Skills*, October 16, 2025. <https://claude.com/blog/skills>

## Earlier in this series

- Post 29: *Calibrate: Follow the Business Value*. <https://lnkd.in/p/gKBZHfWa>
- Post 28: *Calibrate: Working on Agentic AI, or Working with It?* <https://lnkd.in/p/gS5s4eRD>
- Post 15: *Calibrate: Where Does the Capability Live?* <https://lnkd.in/p/g7Es6PC3>
- Post 10: *The CPU Stack Runs on Specification. The LLM Stack Runs on Judgment.* <https://lnkd.in/p/ga3apNfd>
- Post 8: *Knowing Every Layer Is the Job. Building Every Layer Is Not.* <https://lnkd.in/p/gReNQhZ2>
