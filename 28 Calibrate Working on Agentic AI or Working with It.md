# Calibrate: Working on Agentic AI, or Working with It?

*Originally published on LinkedIn: <https://lnkd.in/p/gS5s4eRD>*

## Too much to follow

In September 2026 alone, Anthropic, Google, and OpenAI each released major new models. Between September 1 and 25, Claude Code published 24 releases on GitHub; Codex published at least 100, counting pre-releases. Since May, OpenAI and Anthropic have each set up a company whose job is to put engineers inside customers' businesses.

All of it goes by one name: agentic AI. But it is not one kind of work, and it does not ask the same things of the people doing it.

I follow this field closely, and I still get lost in it. The question I keep coming back to is not "what's new?" but "what deserves my time?"

To calibrate is to step back, re-map the field, and then decide. This article does that in three steps: a map of the work, this month's news placed on the map, and what the map means for where you spend your time.

## The map

Agentic AI work falls on two sides, with a few concerns that run across both.

**Working on agentic AI** means building what other people use.

- **Building agents:** training models for agentic work, and building agent products such as Claude Code, Codex, and Pi.
- **Frameworks and infrastructure:** SDKs and frameworks such as LangChain, protocols such as MCP, and the runtimes agents run in.

**Working with agentic AI** means using agents to get your own work done.

- **Software engineering:** writing, reviewing, testing, and migrating code with coding agents.
- **Business operations:** building agents into support, finance, sales, HR, and other workflows.
- **Science and research:** agents that search, analyze, and run experiments.

**Across both:** evaluation and reliability, security and governance, and cost. These matter whichever side you are on.

One test decides the side: **was it released for others to use, or built for your own work?** Publishing an agent framework on GitHub is working on agentic AI. Building a refund-decision agent into your own order system is working with it, even though you built an agent. Training your own coding agent is working on it, and it puts you in competition with the model vendors.

A second rule follows: **classify the work, not the employer.** An engineer at a model vendor who spends the year inside a customer's claims process is working with agentic AI.

There is no agreed taxonomy of this work; researchers slice it in several ways. This map uses the terms of the past two to three years. The appendix explains how it was built.

## This month's news, on the map

**New models: working on, building agents.** Anthropic released Claude Fable 5.1 on September 1 and Claude Opus 5.5 on September 22. Google released Gemini 3.8 Flash on September 2, its "third Flash release in only six weeks." OpenAI followed GPT-6 Astra, whose system card is dated September 3, with the cheaper GPT-6 Sol and Luna on September 22.

**Coding agents: both sides.** Building Claude Code and Codex is working on agentic AI. Using them to write and review code is working with it, in software engineering. The products are moving from one agent to many: Claude Code can now orchestrate dozens to hundreds of subagents, and separate sessions can message each other.

**Open source: working on, and too big to ignore.** LangChain, one of the best-known agent frameworks, has about 147,000 GitHub stars, and its main Python package was downloaded about 169 million times in the past month. Pi, a minimal agent harness that started in August 2025 under Mario Zechner's own GitHub account, has about 110,000 stars. OpenClaw, an AI agent built with Pi's SDK, has about 391,000, more than twice Claude Code's 148,000. Stars measure attention rather than use, but the attention is real.

**AI FDEs: working with, in business operations.** A forward-deployed engineer (FDE) works inside a customer's business and builds AI into its workflows. On Indeed, FDE job postings in April 2026 were roughly 729% higher than a year earlier [1]. A study by the executive-search firm Christian & Timbers found that 5–10% of companies planned to hire FDEs at the start of 2026, and 70% by the end of the second quarter [2]. In May, OpenAI launched the OpenAI Deployment Company, starting with about 150 FDEs and deployment specialists from its acquisition of Tomoro [3]. In July, Anthropic, Blackstone, and Hellman & Friedman introduced Ode with Anthropic [4]. These companies are tied to model vendors; the work is on the "with" side.

**Restricted cyber models and falling prices: across both.** Claude Mythos 5.1 is the same model as Fable 5.1, with more permissive safeguards, for vetted organizations whose work is affected by Fable's cybersecurity and life-sciences restrictions. Gemini 3.8 Flash Cyber is available to "trusted defenders" through Google's new Fairwind Program. Meanwhile, Anthropic says Opus 5.5 costs about 40% less to run than Opus 5 on typical workloads, and OpenAI says GPT-6 Luna at its maximum setting beats GPT-5.6 Sol at its medium setting, at a tenth of the cost.

## Where do you stand?

The map is the same for everyone. What it means for your time depends on where you stand.

**Is working on agentic AI realistic for you?** That side is concentrated: a handful of model vendors and a limited number of agent and framework companies. Open source is the most accessible way in, since it needs no employer. Pi shows how far a project can travel from one developer's GitHub account.

**If not, is time on Claude Code or Codex wasted beyond using them?** No. They are the most visible working examples of how agents are built: the harness, subagents, permissions, and evals. Companies build their own agents from the same parts. Anthropic even offers Claude Code as a library: its Agent SDK provides the same tools, agent loop, and context management that power Claude Code. Understanding how a coding agent works is preparation for building agents into your own company's workflows.

**FDE work is on the "with" side, but it is where knowledge of how agents are built pays off most.** It carries that knowledge into one company's workflows. Christian & Timbers counts roughly 17,000 FDEs in the U.S., but estimates that only about 2,000 engineers have the full mix of "sector know-how, gravitas, and hands-on applied AI experience" needed to help enterprises consistently see a return on AI [2]. The scarce part is the mix, not the title.

**Most readers will work with agentic AI**, in software engineering or business operations. That is not the lesser side. Few people build any given layer of a stack; many more use it. That is what a maturing stack looks like.

## What lasts

Features change weekly. Claude Code shipped a release nearly every day in September, and Google shipped three Flash models in six weeks. What you learn about a specific feature may be out of date by the time you use it.

Some things keep their value:

- **Judgment about workflows:** which work to hand to an agent, and how the process around it has to change.
- **Evaluation:** telling whether an agent's output is actually right.
- **Security and governance:** deciding what an agent may touch, and who answers for what it does.
- **Cost:** knowing when an agent is worth running.
- **Knowledge of your business:** the part no vendor can ship.

Most of these are the across-both concerns on the map, so they stay useful whichever side you choose. They are also close to what Christian & Timbers found scarce among FDEs: sector know-how and hands-on applied AI experience [2].

**Watch the trends. Invest your time in skills that last and create real business value.**

---

## Post image

| Side | Area | What the work is | Recent examples |
|---|---|---|---|
| Working on | Building agents | Training models for agentic work; building agent products | Claude Fable 5.1 and Opus 5.5, GPT-6, Gemini 3.8 Flash; Claude Code, Codex, Pi, OpenClaw |
| Working on | Frameworks and infrastructure | SDKs, frameworks, protocols, runtimes | LangChain and LangGraph, Claude Agent SDK, MCP |
| Working with | Software engineering | Writing, reviewing, testing, and migrating code with agents | Coding and PR review with Claude Code or Codex |
| Working with | Business operations | Building agents into support, finance, sales, HR, and other workflows | FDE work at the OpenAI Deployment Company and Ode with Anthropic |
| Working with | Science and research | Agents that search, analyze, and run experiments | Research and analysis agents |
| Across both | Evaluation and reliability | Checking that agents do the right thing | Plugin evals in Claude Code |
| Across both | Security and governance | Deciding what agents may touch, and who answers for them | Claude Mythos 5.1, Gemini 3.8 Flash Cyber |
| Across both | Cost | Making agent work affordable at scale | GPT-6 Sol and Luna; Claude Opus 5.5 pricing |

**The test:** released for others to use → working on. Built for your own work → working with. Classify the work, not the employer.

## Appendix: how this map was built

There is no agreed taxonomy of agentic AI work. Even the definition of "agent" is unsettled [5].

The top level adapts the structure of a widely cited 2023 survey, which organizes the field into building agents, applying them, and evaluating them [6]. Here, building becomes "working on," applying becomes "working with," and evaluation moves across both, joined by security and cost. The survey's own terms are left out: it was written before tool protocols such as MCP existed, and before "harness" and "skills" became everyday words.

Newer work includes a systematic review of agentic AI research topics [7].

The second level and the examples come from practice: vendor announcements and documentation for what the leaders ship, GitHub and package registries for what builders adopt, and job-posting data for what employers hire. Discussion on X and LinkedIn shaped the questions but is not counted here.

Scope: roughly September 2023 to September 2026.

## References

[1] Cadie Thompson and Lakshmi Varanasi, *Job postings for this tech role have grown more than 700% in the last year*, Business Insider, updated May 18, 2026, as syndicated by AOL. <https://www.aol.com/articles/job-postings-tech-role-grown-185134480.html>. Indeed data shared with Business Insider: FDE postings in April 2026 stood 5,230% above January 2025 levels, roughly 729% year over year. These are relative levels, not posting counts.

[2] Rebecca Bellan, *Forward-deployed engineers are the AI industry's latest talent obsession*, TechCrunch, July 30, 2026. <https://techcrunch.com/2026/07/30/forward-deployed-engineers-are-the-ai-industrys-latest-talent-obsession/>. Reports a Christian & Timbers study conducted January–June 2026: interviews with more than 250 C-suite hiring executives across 180 companies, a survey of 80 Fortune 500 executives, and interviews with more than 300 FDEs and applied AI engineers. Christian & Timbers is an executive-search firm that recruits for this role.

[3] OpenAI, *OpenAI launches the OpenAI Deployment Company to help businesses build around intelligence*, May 11, 2026. <https://openai.com/index/openai-launches-the-deployment-company/>. Quoted from the version republished by Advent International, a founding partner: <https://www.adventinternational.com/news/openai-launches-the-openai-deployment-company-to-help-businesses-build-around-intelligence/>.

[4] Ode, *Anthropic, Blackstone, and Hellman & Friedman Introduce Ode with Anthropic, an Enterprise AI Services Firm*, July 15, 2026. <https://www.ode.com/press/anthropic-blackstone-and-hellman-friedman-introduce-ode-with-anthropic-an-enterprise-ai-services-firm>. Founding partners, CEO Chris Taylor, and the May 2026 acquisition of Fractional AI. The press release discloses no dollar figure.

[5] OECD, *The Agentic AI Landscape and Its Conceptual Foundations*, OECD Artificial Intelligence Papers, No. 56, February 13, 2026. <https://www.oecd.org/content/dam/oecd/en/publications/reports/2026/02/the-agentic-ai-landscape-and-its-conceptual-foundations_a9d4b451/396cf758-en.pdf>. Pages 8 and 15 discuss competing interpretations of AI agents and the lack of consensus on their defining characteristics.

[6] Lei Wang et al., *A Survey on Large Language Model based Autonomous Agents*, 2023. arXiv:2308.11432. <https://arxiv.org/abs/2308.11432>. Organizes the field into agent construction, application, and evaluation.

[7] Moralles et al., *A Systematic Literature Review of Agentic AI: Definitions, Architectures, and Challenges*, IEEE Access, vol. 14, pp. 36176–36190, February 25, 2026. <https://doi.org/10.1109/ACCESS.2026.3668138>. Reviews 48 studies published between 2021 and 2025 and organizes them into five research fields.

## Earlier in this series

- Post 1: *Write Software That Uses Agentic AI*. <https://lnkd.in/p/gmh8qr5J>
- Post 2: *Agents Break Open Software's Stable Relationship*. <https://lnkd.in/p/gVtniMsP>
- Post 8: *Knowing Every Layer Is the Job. Building Every Layer Is Not.* <https://lnkd.in/p/gReNQhZ2>
- Post 14: *Agentic AI Moves Too Fast. Calibrate.* <https://lnkd.in/p/gGShbrY4>
- Post 15: *Calibrate: Where Does the Capability Live?* <https://lnkd.in/p/g7Es6PC3>
- Post 16: *Calibrate: Choose a Model for Enterprise Refund Decisions*. <https://lnkd.in/p/gk_7yGX5>
- Post 18: *Enterprise Systems Before AI: Academia, Vendors, Architecture, Process, Data, and Operations*. <https://lnkd.in/p/gPgiypEM>
