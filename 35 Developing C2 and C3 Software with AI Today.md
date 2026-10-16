# 35. Developing C2 and C3 Software with AI: Today

*Originally published on LinkedIn: <https://lnkd.in/p/gjq3THpk>*

Post 32 sorted software into three categories by which stack carries its business function. In C1, code on the CPU stack carries it. In C2, the LLM carries it; any code only supports it. In C3, both stacks carry part of the business function, and judgment crosses into business logic.

Post 33 asked how AI is changing the development of C1 software. It said C2 and C3 would follow in Posts 34 and 35. Post 34 first defined the LLM stack, because describing how software for that stack is developed needed the vocabulary. This post takes C2 and C3 together and maps how a business develops both for its own work today, including what AI takes on in building and improving them. They are built from many of the same parts, and one post keeps the overlaps in view. The short version: C2 and C3 share development methods, C3 adds boundary engineering, and both depend on checking.

## A field still taking shape

AI moves quickly, with many people building models, products, and ways of working at once. The work and the terms used for it are still taking shape. Three examples show why the names alone give us too little orientation.

**"AI engineer" can name two different jobs.** An AI engineer can build models that others use, or build on those models for a business's own work, such as a refund-decision agent in its order system. One title covers both, though the work serves different people.

**An "AI agent" can be mostly ordinary code.** A product sold as an agent can be ordinary code with LLM calls at a few points, or a model that directs nearly everything. The name doesn't say which stack carries the business function. Mixing code and LLM calls isn't enough for C3. Code that runs the model only supports the business function; code that checks a refund carries business logic. The software is C3 only when code carries business logic and judgment crosses into it.

**"Vibe coding" says how software is built, not what runs it.** The term describes AI writing code from prompts in plain language. A vibe-coded app can be C1, and an agent's instructions can be written by hand. AI that helps build software and AI that runs it play different roles.

**Finding our way requires distinctions these terms don't make: who the work is for, which stack carries the business function, and what role AI plays.**

## Four lenses and a map

To calibrate (Posts 14 and 28), I step back and look at the work through four lenses, all from earlier posts: *on or with AI* from Post 28, the other three from Post 32.

| Lens | Question | Sorts |
|---|---|---|
| *On or with AI* | Was it released for others to use, or built for your own work? | Activities |
| *Build or run software* | Is AI helping build the software, or running it? | Activities |
| *CPU or LLM stack* | Does this part run on the CPU or on the LLM? | Software and its parts |
| *Business or supporting function* | Does this part carry the business function, or support it? | Software and its parts |

Post 32 also gave a test for *business or supporting function*: would a piece of code still mean something to the business if the model were removed?

Together, the two lenses that sort software give C1, C2, and C3: they ask which stack carries the business function. The other two set the scope: software a business builds for its own work, and what AI takes on in building and improving it. The AI that runs the software comes in only as the reason a method is needed.

Most of the evidence here is published methods and vendor guidance, so the map shows what is available today more than how widely businesses use it.

Who develops is a wider group than in Post 33, which looked inside the development team. Much of what gets built for C2 and C3 is language, and people on the business team can write it themselves. So this post includes anyone who creates, configures, or improves the language and code. When one person writes the requirements, builds the software, and accepts it, the checks have to be made explicit.

The map has four areas. The first two follow Post 34's two levels, and the boundary is what C3 adds.

| Area | What gets built | C2 | C3 |
|---|---|:---:|:---:|
| The LLM call | Instructions, knowledge, skills, and the composition of each context | ● | ● |
| The agent call | Harnesses, tools, and the arrangement of the loop | ● | ● |
| The boundary | Placement of business rules, tools that act on business systems, processes that call agents, and limits | | ● |
| Checking | Acceptance criteria, eval suites, tests, and outcome checks | ● | ● |

Each area answers three questions: what AI takes on, what people supply, and what engineering makes it work today.

**Two lenses classify the software. Two set the scope: software built for your own work, and AI's role in building it.**

## The LLM call: what the model reads

- **What AI takes on:** drafting and revising instructions and skills, and testing them against sample tasks. In research, optimizers rewrite prompts against a measure.
- **What people create, decide, and supply:** the instructions, the knowledge, and the skills; what each call should see; and the domain knowledge needed to judge the results.
- **What engineering makes it work today:** composing each context within a limited budget, and packaging knowledge so that it loads only when it's needed.

Post 34 defined the LLM call as the LLM processor's only operation: it "reads one context and generates one response." Everything a C2 or C3 system tells the model arrives in that context. So the first thing to build is language: instructions, knowledge, and skills. The harness then composes each context from them, which Post 34 called composing the message.

Skills package that language. Anthropic describes Agent Skills as "organized folders of instructions, scripts, and resources that agents can discover and load dynamically," designed around "progressive disclosure": the agent loads a skill's details only when a task calls for them. Anthropic published the format as an open standard in December 2025, and says "anyone can now specialize their agents with composable capabilities" [1]. One skill can hold language and code together, and the lenses still apply inside it. A script that extracts text from a PDF so the model can read it supports the work; a script that computes a refund carries business logic.

Composing the context is engineering in its own right. Anthropic's Applied AI team defines context engineering as "the set of strategies for curating and maintaining the optimal set of tokens (information) during LLM inference," and calls context "a critical but finite resource" [2]. Its techniques include compacting long histories, keeping structured notes, and handing subtasks to sub-agents with their own contexts [2]. The code that composes each context is supporting code: it would mean nothing to the business without the model. But its effects show up in the model's judgment, so tests alone can't check it.

AI already takes on part of the building. Anthropic's skill-creator, a skill for making skills, has the agent draft "2-3 realistic test prompts," run each one with and without the skill, and "Rewrite the skill based on feedback from the user's evaluation of the results" [3]. The AI drafts, runs, and revises; a person judges. Research goes further: GEPA has an LLM reflect on a system's runs "in natural language to diagnose problems, propose and test prompt updates" [4]. Both are build work, even when a running agent does it.

People supply knowledge of the business. In the OECD's interviews with 25 organizations, a recurring theme was "the essential role of domain expertise in defining task scope, identifying failure modes, and validating outputs" [5].

The comparison with C1 helps here. In C1, a spec says what the code must do, and the code can be checked against the spec's stated properties. In C2 and C3, the language is itself what runs, read by judgment. Evaluation can still check stated properties of the output, such as its format, with code. It also grades the judgment those checks leave unresolved.

**The language is part of the software. AI helps write and test it; people supply the business knowledge and judge the results.**

## The agent call: harnesses, tools, and how the loop is arranged

- **What AI takes on:** writing harness and tool code, as coding agents do for any code; generating test tasks for tools; and refactoring tools from evaluation transcripts.
- **What people create, decide, and supply:** whether to start from a packaged harness or build one; which tools the agent gets and how they are described; and how much of the loop to leave to the model.
- **What engineering makes it work today:** tools designed for an agent to use, and feedback inside the loop that the agent can check its work against.

Post 34 described the agent call as "one call to an agent on a task." Underneath, "a harness loops through LLM calls and tool calls until the model gives a final response or the harness stops it." The harness and the tools' implementations are code. The tools' descriptions are language the model reads, and some decisions in the loop are left to the model's judgment.

Teams can start from a packaged harness. Anthropic says "the agent harness that powers Claude Code (the Claude Code SDK) can power many other types of agents, too," including agents for finance, customer support, and research [6]. The same post renamed it the Claude Agent SDK. Other teams build their own: HumanLayer's *12-Factor Agents* guide argues for owning the prompts, the context window, and the control flow [7]. Either way, the harness is supporting code, so the choice doesn't change the category.

The loop can be arranged in different ways. Anthropic distinguishes workflows, "systems where LLMs and tools are orchestrated through predefined code paths," from agents, "systems where LLMs dynamically direct their own processes and tool usage," and recommends "finding the simplest solution possible" [8]. Here, I group both under Post 34's agent call. They differ in how much of the context selection, loop termination, and routing they leave to the model. The arrangement alone doesn't decide the category. If a chain's code arranges LLM calls that draft, critique, and revise a report, it supports the model's work, and the software stays C2. If a chain's code applies the business's own approval thresholds, it carries business logic, and the software is C3.

Tools get particular attention. Anthropic calls them "a new kind of software which reflects a contract between deterministic systems and non-deterministic agents" [9]. In its process, Claude Code generates evaluation tasks, agents run them, and Claude Code refactors the tools from the transcripts, with held-out test sets to guard against overfitting. By Anthropic's own measurements, tools refined this way improved even on versions written by its experts [9]. Developers also build feedback into the loop. Anthropic's SDK guidance prefers "clearly defined rules for an output" to having another model judge it [6].

The comparison with C1 applies to the code. Coding agents write harness and tool code, and Post 33's methods check it: specs, tests, and review. But correct code answers only half the question. A harness can pass every test and still compose contexts that lead the model to poor judgments. Evaluation answers the other half.

**The harness is code, but whether the agent call works shows in the model's results.**

## The boundary: where judgment crosses into business logic

This area belongs to C3. Post 32 said: "C3 asks you to design the boundary: who owns the loop, what crosses it, and what limits a wrong judgment." Before those three questions comes a decision about each business rule: should it live in language, applied by judgment, or in code, enforced by specification?

- **What AI takes on:** writing the code on the business side of the boundary, such as tools and process steps, and the tests for it.
- **What people create, decide, and supply:** where each business rule lives, who owns the loop, what may cross and in what form, and which actions need a person's approval.
- **What engineering makes it work today:** tools that validate what crosses and enforce the business's constraints and access controls, risk ratings on tools, approval checkpoints, and other limits that contain a wrong judgment.

**Where each rule lives.** OpenAI's guide recommends agents for work involving "nuanced judgment, exceptions, or context-sensitive decisions, for example refund approval," and for systems that "have become unwieldy due to extensive and intricate rulesets." Otherwise, it says, "a deterministic solution may suffice" [10]. Rules also move the other way: "Work can move from judgment to specification as people learn to specify it" (Post 32). Where a rule lives determines how it is enforced. Code enforces specified behavior, and tests check its implementation. An LLM applies a rule written in language through judgment.

**Who owns the loop.** In LLM-led C3, the agent acts on business systems through tools. The Model Context Protocol (MCP) says "Tools enable models to interact with external systems, such as querying databases, calling APIs, or performing computations." Its specification requires servers to "Validate all tool inputs" and "Implement proper access controls," and lets a tool report "Business logic errors" back to the model as "actionable feedback that language models can use to self-correct" [11]. The code behind the tool enforces the business rules; the model's judgment proposes what to do.

In code-led C3, a business process calls the agent. Camunda, which sells process orchestration software, scopes an agent in a business process "as a subprocess—specifically an ad hoc subprocess." Inside it, the model "focuses entirely on the reasoning related to how the process should proceed," while "the process engine is still responsible for doing the actual execution of those tasks" [12]. The process around the agent carries the business logic. As Post 32 noted, the two forms can nest. Here a code-led process hands one step to an LLM-led loop.

**What crosses.** A message crosses the boundary: a tool call going into business code, or an agent's result coming back to a process. The LLM call's partial contract (Posts 10 and 34) "covers the format of the response, not the judgment inside it." Code at the boundary can go further and enforce the business's specified constraints, including explicit eligibility rules: a refund must be positive, can't exceed the order total, and needs approval above a set amount. What code can't settle is whatever those rules leave to judgment, such as how much weight a customer's explanation deserves. Post 11 put it this way: "Vendors can enforce what crosses the edge. They cannot tell code whether the judgment inside it was right."

**What limits a wrong judgment.** So people also build limits. OpenAI's guide advises rating each tool "low, medium, or high" by "read-only vs. write access, reversibility, required account permissions, and financial impact." High-risk actions, such as "canceling user orders, authorizing large refunds, or making payments," "should trigger human oversight until confidence in the agent's reliability grows" [10]. In the OECD's interviews, "No participating organisation reported deploying agentic AI systems with unrestricted autonomy." Autonomous execution was typically limited to defined task scopes, with human approval for consequential or irreversible actions such as payments and data deletion. Approval thresholds varied across organizations [5].

**At the boundary, people decide where each rule lives, code enforces what can be specified, and limits contain the rest.**

## Checking: judgment, specified behavior, and their interaction

- **What AI takes on:** grading outputs as a model-based judge, and generating test tasks.
- **What people create, decide, and supply:** explicit acceptance criteria, tasks drawn from real failures, calibration of model graders, and acceptance itself.
- **What engineering makes it work today:** evaluation harnesses that run repeated trials, tracing, checks of business state, and monitoring in production.

Three kinds of checks run together.

**Judgment is checked by evaluation.** Anthropic's guide to agent evals combines "code-based, model-based, and human" graders [13]. Because model outputs vary, Anthropic runs several trials per task, and its pass^k metric "measures the probability that all k trials succeed." A suite can start small: "20-50 simple tasks drawn from real failures is a great start" [13].

**Specified behavior is checked as in C1.** The harness, the tools, and the business code have tests.

**Their interaction is checked through outcomes.** In one of Anthropic's examples, "the outcome is whether a reservation exists in the environment's SQL database"; other graders inspect ticket status and refund records [13]. Properties like these can be checked exactly, and so can others a requirement states, such as the requested flight or explicit eligibility rules. One eval suite can check these with code-based graders and leave the remaining judgment to model-based or human graders. Outcome checks aren't only for C3: a report produced by C2 software can have checkable properties too. What C3 adds is checking what judgment did in the business systems that code governs.

The guide also advises "Converting user-reported failures into test cases" [13], as C1 teams turn bugs into regression tests.

Who writes the checks? At Anthropic, a dedicated team owns the evaluation infrastructure, while "domain experts and product teams contribute most eval tasks and run the evaluations themselves" [13]. That fits this post's wider set of contributors, but it means one person may write the requirement, the language, and the grader, and then accept the result.

Each part of checking contributes something different. Explicit criteria and evaluation cases make acceptance checkable; Anthropic's test of a good task is whether "two domain experts would independently reach the same pass/fail verdict." Another reviewer adds independence where the stakes call for it. Monitoring in production adds later evidence and improves the next iteration, but it doesn't replace acceptance. Anthropic presents these methods as complementary: "automated evals for fast iteration, production monitoring for ground truth, and periodic human review for calibration" [13].

How far does practice reach today? LangChain's survey of 1,340 respondents found that nearly 89% had implemented observability for their agents, but only 70.5% ran any evaluation: 52.4% ran offline evaluations and 37.3% ran online evaluations, and some ran both [14]. Among respondents with agents in production, 94% had some observability, while 77.2% ran some evaluation. Among evaluation methods, human review (59.8%) led LLM-as-judge (53.3%) [14]. In the OECD's interviews, evaluation tooling for agents "remains significantly less mature than for standalone LLMs," and organizations relied on "task-specific proxies or client-defined KPIs" [5].

**Tests check code; evals check stated outcomes with code-based graders and assess remaining judgment with model-based or human graders.**

## What the map shows

C2 and C3 share three of the four areas: the LLM call, the agent call, and checking. C3 adds the boundary, along with checks of how judgment and business logic interact. Methods cross the areas: Post 33's methods build and test the code wherever it sits, evaluation checks both the language and the harness's effect, and one skill can hold language and code together. The categories sort software, not methods.

My reading: the evidence suggests that evaluation trails visibility into what agents do. In LangChain's survey, observability runs ahead of evaluation. In the OECD's interviews, evaluation tooling for agents lags the tooling for standalone LLMs. Both sources have limits: a vendor's survey of self-selected respondents, and 25 interviews the OECD calls illustrative. Seeing what agents do doesn't establish whether their work meets the business's acceptance criteria. Closing that gap takes development work in both C2 and C3.

Later posts can go deeper into particular methods on this map.

**C2 and C3 share development methods. C3 adds boundary engineering. Both depend on checking.**

---

## References

[1] Barry Zhang, Keith Lazuka, and Mahesh Murag, *Equipping agents for the real world with Agent Skills*, Anthropic, October 16, 2025; updated December 18, 2025, when Agent Skills was published as an open standard. <https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills>

[2] Prithvi Rajasekaran, Ethan Dixon, Carly Ryan, and Jeremy Hadfield (Anthropic's Applied AI team), *Effective context engineering for AI agents*, Anthropic, September 29, 2025. <https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents>

[3] Anthropic, *skill-creator*, in the anthropics/skills GitHub repository, accessed October 2026. <https://github.com/anthropics/skills/tree/main/skills/skill-creator>. The repository says its skills are "provided for demonstration and educational purposes only."

[4] Lakshya A. Agrawal et al., *GEPA: Reflective Prompt Evolution Can Outperform Reinforcement Learning*, arXiv:2507.19457, revised February 14, 2026; accepted at ICLR 2026. <https://arxiv.org/abs/2507.19457>. Research on benchmark tasks, not business deployments.

[5] OECD, *Agentic AI in organisations: Early insights from practitioner interviews*, OECD Artificial Intelligence Papers, No. 65, September 2026. <https://www.oecd.org/content/dam/oecd/en/publications/reports/2026/09/agentic-ai-in-organisations_1f17d235/1257a26f-en.pdf>. Semi-structured interviews with 25 organizations; the report says its findings "should be interpreted as illustrative rather than representative."

[6] Thariq Shihipar, *Building agents with the Claude Agent SDK*, Anthropic, September 29, 2025. <https://claude.com/blog/building-agents-with-the-claude-agent-sdk>. Guidance from the SDK's vendor.

[7] Dex Horthy, *12-Factor Agents*, HumanLayer, GitHub repository, accessed October 2026. <https://github.com/humanlayer/12-factor-agents>. A practitioner's design guide; undated.

[8] Erik S. and Barry Zhang, *Building effective agents*, Anthropic, December 19, 2024. <https://www.anthropic.com/engineering/building-effective-agents>. Older than the other sources here, and still a common reference for the distinction between workflows and agents.

[9] Ken Aizawa, *Writing effective tools for agents — with agents*, Anthropic, September 11, 2025. <https://www.anthropic.com/engineering/writing-tools-for-agents>. The comparisons with expert-written tools are Anthropic's own, on its internal Slack and Asana tools, and appear only as charts.

[10] OpenAI, *A practical guide to building agents*, undated PDF, accessed October 2026. <https://cdn.openai.com/business-guides-and-resources/a-practical-guide-to-building-agents.pdf>. Guidance from a model vendor.

[11] Model Context Protocol, *Specification, version 2025-11-25: Tools*, accessed October 2026. <https://modelcontextprotocol.io/specification/2025-11-25/server/tools>

[12] Niall Deehan, *Key Principles in Designing Agents for Production – Part I: Camunda's Agent Architecture Fundamentals*, Camunda, May 29, 2026. <https://camunda.com/blog/2026/05/key-principles-in-designing-agents-for-production-part-1/>. Camunda sells process orchestration software.

[13] Mikaela Grace, Jeremy Hadfield, Rodrigo Olivares, and Jiri De Jonghe, *Demystifying evals for AI agents*, Anthropic, January 9, 2026. <https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents>. The division of eval work is Anthropic's account of its own teams; the rest is guidance.

[14] LangChain, *State of Agent Engineering*. <https://www.langchain.com/state-of-agent-engineering>. A public survey of 1,340 respondents, November 18 to December 2, 2025; self-reported, with 63% of respondents in technology and 49% at organizations under 100 people. The page is dated June 12, 2026. LangChain sells LangSmith, an observability and evaluation product.

## Earlier in this series

All posts are archived in the GitHub repository: <https://github.com/young-rex/notes-on-agentic-ai>

- Post 34: *Defining the LLM Stack: One Operation and Messages* <https://lnkd.in/p/gBRCT5R6>
- Post 33: *Developing C1 Software with AI: Today and the Path Ahead* <https://lnkd.in/p/gE9xvGSd>
- Post 32: *AI Coding Has Two Sides. Ours Starts with What Runs the Software.* <https://lnkd.in/p/gacN7SgW>
- Post 28: *Calibrate: Working on Agentic AI, or Working with It?* <https://lnkd.in/p/gS5s4eRD>
- Post 14: *Agentic AI Moves Too Fast. Calibrate.* <https://lnkd.in/p/gGShbrY4>
- Post 11: *The Models Are Getting Better. Code Still Cannot Fully Check Their Judgment.* <https://lnkd.in/p/gQqVvCsF>
- Post 10: *The CPU Stack Runs on Specification. The LLM Stack Runs on Judgment.* <https://lnkd.in/p/ga3apNfd>
