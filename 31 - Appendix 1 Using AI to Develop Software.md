# Using AI to Develop Software: Adoption, Practices, and Evidence (Research Findings)

- **Date:** September 30, 2026
- **Appendix 1 to:** Post 31, "AI Coding Has Two Sides. Ours Is Too Big for One Post." (Notes on Agentic AI)
- **Author:** Rex Young
- **License:** [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)

## Scope and method

- **Scope:** using AI to develop software. The resulting software may be conventional or may contain AI.
- **Out of scope:** building the AI coding capability itself (training coding models, building coding agents and tools; tools appear only as they're used), and AI used outside software development.
- **Segments:** none is singled out; enterprise, startups, open source, and other segments sit side by side.
- **Geography:** sources that are global or about the US; sources limited to another country are excluded.
- **Framing:** no external framework applied; categories use the sources' own labels.
- **Coverage:** broad on purpose and not ranked; inclusion doesn't make an item important. The source-type tags below help weigh each item.
- **Synthesis:** the enterprise note in section 1, section 3e, and sections 5 and 6 are this report's own conclusions; the rest summarizes the sources.
- **Tags:** [R] research · [S] industry surveys, usage data, research programs · [A] analysts and consultancies · [G] government, standards bodies, foundations · [V] vendor-published · [P] practitioners and press. "Via" marks secondary coverage. Vendor figures are self-reported; vendor product documentation shows what is offered, not adoption or outcomes.

## 1. Who uses AI coding, and on what software

- **Professional developers, overall:** 2025: 84% use or plan to use AI; 14% use agents daily at work; 38% have no plans to adopt AI agents. May–July 2026: 90% use coding agents at work weekly, 68% daily (15,000+ developers). About 42% of committed code is AI-generated; developers expect about two-thirds by 2027 [25][26][28][29].
- **Big tech, internal:** Microsoft: 20–30% of code AI-generated (April 2025). Google: reported at about 75% of new code (April 2026; secondary). Amazon: Java upgrades saved 4,500 developer-years (company figure). Google: LLM-assisted migrations took about 50% less time (developer estimate) [92][93][64][11].
- **Startups:** 25% of Y Combinator's Winter 2025 startups had ~95% AI-generated code. 2025: 33% of Claude Code conversations served startup work, 13% enterprise work [91][59].
- **Enterprises, all industries:** about 2 in 10 organizations scale coding agents (31% of large firms); 32% skipped buying at least one software product because agentic coding could build it (survey May–June 2026, 1,719 respondents). Gartner: enterprise coding-agent market $9.8–11B annualized (April 2026); 90% of enterprise engineers will use AI code assistants by 2028 (2025 Magic Quadrant). The 2026 Magic Quadrant replaced "AI Code Assistants" with "Enterprise AI Coding Agents," solutions that "perceive context, translate human intent into multistep plans, and execute and verify those steps across code, tests and related engineering artifacts"; Leaders: Anthropic, Cursor, GitHub, OpenAI; more than 70% of enterprise software engineers will rely on AI coding agents for both synchronous and asynchronous work by 2028. Coding is 55% of departmental AI spending (2025). About 90% of 500+ technical leaders' organizations use AI for coding [44][47][48][53][62].
- **Financial services:** Goldman Sachs piloting hundreds of Devin instances as a "hybrid workforce" (2025) [94].
- **Enterprise software platforms:** Salesforce Agentforce Vibes: Apex and Lightning code aware of the customer's schema, plus tests and deployment; 1M+ AI-written lines accepted into production (vendor). SAP Joule for Developers: ABAP generation from an SAP-trained model, unit tests, explanation, custom-code migration; SAP cites IDC: 80% of developers report higher productivity, 35% average gain. SAP Integration Suite: integration flows from natural language [65][73][74].
- **Data engineering and data science:** Databricks Genie Code (agent mode): builds medallion pipelines from a prompt, runs them, fixes errors, asks approval before running code; migrates dbt and Informatica projects (beta). Beyond pipelines, evidence is thin; a vendor report says agentic coding is spreading to data science [75][61].
- **Legacy modernization and large-scale transformation:** AWS Transform custom: define a transformation once, pilot it, then run it across many repositories (language, framework, API, library, and architecture migrations). AWS, IBM, Microsoft, and Anthropic all offer agents for COBOL; IBM shares fell more than 13% in a day after Anthropic's COBOL post (February 2026) [76][78][68][67][66][99].
- **Industrial automation:** Siemens: TIA Portal agents generate, adapt, test, and fix PLC code (SCL, LAD), build HMIs, and configure drives and networks; shown in beta at SPS 2025. Research: vendor-specific PLC code for Siemens and CODESYS, 90%+ compiles on 914 tasks [77][100][15].
- **Non-developers ("vibe coders," citizen developers):** Lovable went from $1M to $100M annual recurring revenue in eight months, and Replit from $10M to $100M in nine months (both self-reported). Gartner: 40% of new enterprise production software built with vibe coding by 2028; AI-assisted low-code moves app building to business teams. A Replit agent deleted a production database (July 2025) [79][103][50][51][95].
- **Open source:** 932,791 agent-authored pull requests across 116,211 repositories (five agents). Policies across 77 organizations: ban, require disclosure, or accept. Godot banned agent-written and vibe-coded contributions (June 2026). GitHub considering pull-request controls against "AI slop" [10][39][97][98].
- **Security (defensive):** DARPA AIxCC: autonomous systems found 54 planted vulnerabilities, patched 43, and found 18 real ones. Google DeepMind CodeMender upstreamed 72 fixes [57][69].
- **Scientists who program:** 868 surveyed: use highest among students and less experienced programmers; general chatbots preferred; perceived productivity tracks code accepted, not validation [12][13].
- **Embedded and safety-critical:** 80.5% of embedded teams use AI; 83.5% have shipped AI-generated code to production. ISO 26262, IEC 62304, and DO-178C assume human-written, traceable code [36][14].
- **Hardware design (RTL):** spec-to-RTL multi-agent systems (NVIDIA); Cadence RTL agent (vendor claims); literature review [16][17][70].
- **Games:** 36% of game-industry professionals use generative AI; 59% of game programmers view it unfavorably (2026) [37].
- **Infrastructure and DevOps:** survey of 510 engineers on agents in infrastructure work (2026) [38].
- **Education:** only design research on AI coding assistants for learning [19].

**Enterprise note:** AI coding in enterprises extends well beyond new applications, to extensions, integrations, data pipelines, operational software, testing, maintenance, and modernization [73][74][75][76][77][100]. These sources are vendor documentation, so they show scope, not adoption or results.

## 2. Which activities and what code

- Research surveys map agent uses across the whole lifecycle [7]; a 2026 mapping study finds them concentrated in coding, testing, DevOps, and maintenance [8].
- A research taxonomy of AI-for-software-engineering tasks: code generation; code transformation (refactoring, migration and translation, optimization); testing and program analysis (testing, analysis, repair); maintenance (documentation, pull-request review, code understanding); scaffolding; and formal verification. Tasks are characterized by scope (function to whole project), logical complexity, and human intervention (from no AI to AI as an agent) [21].
- Coding and testing are the most mature uses; planning and requirements lag [52][8].
- Adoption differs across testing, review, CI/CD, and incident response; the most-used stages aren't always the most valuable [30].
- Inside Anthropic: debugging and understanding code first, then features; engineers fully hand off 0–20% of their work, mostly tasks that are easy to check or boring; 27% of assisted work wouldn't have been done otherwise [60].
- 2025: web and UI work dominated Claude Code use [59]. TypeScript became GitHub's top language, partly tied to agent-assisted coding; 1.1M+ public repositories import an LLM SDK [71].
- The clearest large-scale gains come from well-specified, checkable work such as migrations and upgrades [11][64].
- DORA: connecting AI to internal codebases, architecture diagrams, wikis, documentation, style guides, metrics, and logs ("context engineering") is a "statistically significant multiplier for individual effectiveness and code quality." DORA's own wording for this context: "architecture, standards, and business logic" [33].
- Gartner (May 2026): enterprises are moving "from AI-assisted development to agentic software development," from planning through review. By 2027, over 65% of teams using agentic coding will treat the IDE as optional [47].

## 3. How it's applied

### 3a. Published models of autonomy and interaction

- **Barke et al., 2023:** acceleration mode (AI executes what you planned) and exploration mode (AI helps you plan) [4].
- **Yegge, 2025:** six waves: traditional coding, completions, chat, agents, clusters of agents, fleets of agents [84].
- **Karpathy, 2025:** "Partial autonomy apps" with an "autonomy slider" [80].
- **Shapiro, 2026:** levels 0–5: autocomplete, intern, junior developer, developer (human reviews everything), engineering team (human writes specs and manages), "dark factory" (no human involvement) [83].
- **Vibe-coding survey, 2025:** five development models: unconstrained automation, conversational iteration, planning-driven, test-driven, context-enhanced [6].
- **Hassan et al., 2025:** "SE 3.0": software engineering for humans and for agents, with separate environments for commanding agents and for agents doing the work [5].
- **Forrester:** "TuringBots" for each role in the development lifecycle, becoming agentic and coordinated [52].
- **Gartner:** "AI-native software engineering" (Hype Cycle, 2025); a shift from AI-assisted to agentic development [49][47].

### 3b. Named practices

- **Vibe coding:** accepting AI output without reading the code. Simon Willison's disciplined counterpart is **vibe engineering**. Karpathy's successor term (February 2026) is **agentic engineering** [82][81].
- **Spec-driven development:** specifications act as the contract between humans and agents [24]. Tools: Kiro, spec-kit, Tessl, OpenSpec [85][86]; BMAD [102]. The evidence is still mostly practitioner reports, talks, and tooling rather than peer-reviewed studies [24]. a16z describes today's workflow as plan → code → review [54].
- **Harness engineering:** an OpenAI team shipped a product with no hand-written code over five months ("Humans steer. Agents execute.") [63]. Thoughtworks recommends wiring automatic checks (compilers, linters, tests, mutation testing) into the agent's loop [86].
- **Software factory:** at StrongDM, no human writes or reviews code; specifications and scenarios drive the agents [87].
- **Delegated cloud agents:** GitHub Copilot cloud agent takes tasks from issues, the agents panel, VS Code, pull-request comments, or schedules; works in an ephemeral GitHub Actions environment; plans, edits a branch, runs tests and linters, and returns a pull request for review. Listed tasks: bug fixes, incremental features, test coverage, documentation, technical debt, merge conflicts. Limits: one repository per session, 59 minutes per run [72]. Agents running overnight and on weekends [45].
- **Repeatable transformations:** AWS Transform custom defines a transformation (stored as a skill) from prompts, documentation, and code samples; pilots it; runs it in bulk across repositories with a build or validation command; and extracts "lessons" from each run for developers to review [76].
- **Agent teams:** one coordinating agent with specialist agents [61]; research prototypes simulate a whole software company (ChatDev, MetaGPT) [18].
- **Keeping developers in control:** experienced developers keep control of design and implementation to protect quality [9].
- **Security guidance:** OpenSSF's instruction files for AI code assistants [55]; OWASP's top risks for agentic applications [56].

### 3c. Tool types

Inline completion; general chatbots (most used in 2025, and preferred by scientists) [27][12]; agents in the IDE, the terminal, and the cloud, and platforms that coordinate several agents [101]; AI code review; app builders that work from a prompt, such as Lovable, Replit, and Bolt [96][101]; builders inside enterprise platforms (Salesforce, SAP, Databricks, Siemens) [65][73][75][77]; specialized tools for mainframes, PLCs, chip design, and security. Agent tools lean toward automation over assistance: 79% of Claude Code conversations versus 49% on Claude.ai [59]. Tool use at work in 2026: Claude Code 39%, GitHub Copilot 21%, Codex 16%, Cursor 12% [26]. AGENTS.md (project-specific guidance for coding agents) and MCP (connections to tools, data, and applications) sit under the Linux Foundation's Agentic AI Foundation, formed in December 2025 [58].

### 3d. Organizational practices

- **Mandates:** Shopify made "reflexive AI usage" a baseline expectation [88]. Coinbase gave engineers a week to adopt AI tools and fired some who didn't; about 40% of its daily code is AI-generated [89][90].
- **Measurement:** DX's framework tracks usage, impact, and cost, and warns against counting accepted suggestions [35].
- **Capabilities:** DORA's seven: clear and communicated AI stance; healthy data ecosystems; AI-accessible internal data; strong version control practices; working in small batches; user-centric focus; quality internal platforms [31][32].
- **Process redesign:** writing and testing code is 25–35% of the time from idea to launch; typical gains are 10–15%, and 25–30% with end-to-end redesign [46]. McKinsey sees smaller teams supervising agents [45].
- **Build versus buy:** 32% skipped a software purchase because agentic coding could build it [44].
- **Governance:** vibe coding and citizen development [50][51]; open-source contribution policies [39]; licensing, where 68% of audited codebases had license conflicts and AI-generated code is cited as a cause [43]; 35% of developers use personal accounts for AI coding tools [29].

### 3e. Ways of working (this report's synthesis)

Not from the sources: four ways, plus the far end of way 2.

- **Interactive assistance (way 1):** a developer works with the AI in a session: completion, chat, edits in the IDE or terminal; the developer reviews along the way. Evidence: most reported use [25][26]; acceleration and exploration modes [4]; partial autonomy [80]; Shapiro levels 0–3 [83]; Gartner's synchronous agent work [48].
- **Delegated task (way 2):** a task or issue is handed to an agent that works in its own environment and returns a pull request for review. Evidence: GitHub cloud agent [72]; 932,791 agent-authored pull requests [10]; Goldman Sachs [94]; overnight runs [45]; fully delegated work is 0–20% at Anthropic [60]; Gartner's asynchronous agent work [48].
- **Validated transformation at scale (way 3):** a change is defined once, piloted, validated, then applied across many modules or repositories. Evidence: AWS Transform custom [76]; Google migrations [11]; Amazon Java upgrades [64]; COBOL [66][67][68]; SAP custom-code migration [73]; Databricks dbt/Informatica migration [75].
- **Application generated from a description (way 4):** someone, often not a developer, describes an app and gets a running one; the code often goes unread. Evidence: Lovable, Replit [96]; Salesforce Agentforce Vibes [65]; Gartner on vibe coding and low-code [50][51]; Willison's definition [82]; Replit incident [95].
- **Far end of way 2:** agents write and check code; no human writes or reviews it. Evidence: StrongDM [87]; OpenAI harness engineering [63].

The ways overlap. Ways 1 and 2 differ by autonomy, way 3 by size of change, way 4 by who does the coding. The published models in 3a sort mainly by autonomy.

## 4. Evidence on outcomes

- **Randomized trials:** a 26% increase in completed tasks across 4,867 developers, using autocomplete-era Copilot; less experienced developers gained more [1]. Experienced open-source developers were 19% slower but believed they were 20% faster [2]. The 2026 follow-up found an 18% speedup for returning developers and 4% for new ones; METR calls this unreliable because many developers refused to work without AI, and is redesigning the study [3].
- **Usage data:** 21% more tasks completed and 98% more pull requests merged, but review time rose 91%, and there was no gain at company level [34]. At one company with a mandate to double output per engineer, the share of pull requests with a human-written review comment fell from about 39% to 21%, while the share with an automated AI review rose from about 19% to 84% [23].
- **Surveys:** throughput is up, but so is instability [31]. 66% get answers that are "almost right, but not quite"; 46% distrust AI tools, 33% trust them, and only 3% highly trust them [25]. 96% don't fully trust AI-generated code, but only 48% always check it [29]. SAP cites IDC: a 35% average productivity gain (vendor-cited) [73].
- **Quality and security:** only about 55% of generation tasks produce secure code; in 45%, the model introduces a known flaw. The rate has stayed essentially flat for two years, and model size makes little difference [40]. AI-written pull requests had about 1.7× more issues [41]. Duplicated code grew eightfold in 2024 [42]. 68% of audited codebases had license conflicts, with AI-generated code cited as a cause [43].
- **Organization level:** only about a quarter of companies report meaningful acceleration, and productivity fell in about 30% after adopting agentic tools [45]. Bain: typical gains of 10–15%, 25–30% with redesign [46].
- **Jobs:** employment of 22–25-year-olds in the most AI-exposed jobs, including software development, is 19% below that of less-exposed peers, up from a 13% gap [20]. A 2026 review reports evidence of skill atrophy among developers using AI coding tools [22].
- **Vendor-reported:** 4–8-month projects finished in two weeks [61]; work done in a tenth of the time [63]; $260M a year saved [64].

## 5. Where sources agree

1. Nearly all professional developers use AI, and agents are where use is growing [25][26][28][31].
2. Review, testing, and integration are the new bottleneck, so individual gains don't automatically become organization-wide gains [31][34][45][46][29].
3. Trust is low and isn't rising with use [25][29][9].
4. Results depend more on the organization (platforms, processes, data) than on the tool; DORA calls AI "an amplifier, magnifying an organization's existing strengths and weaknesses" [31][32][46][45].
5. The clearest wins come from well-specified work that is easy to check [11][64][60].
6. Without controls, quality, security, and licensing risks increase [40][41][42][43][55][56].
7. Coding and testing lead; planning and requirements lag [52][8][30].
8. Supplying internal context and specifications is a main lever for results [33][63][85][86][55].

## 6. Where sources disagree

1. **How big the productivity effect is, and whether it's positive at all:** +26%, −19%, +4% to +18% (flagged as unreliable), +10–15%, falls in 30% of companies, and vendor claims of 10× [1][2][3][46][45][63]. One review conjectures that gains are real on new code but shrink or reverse on mature codebases, which would account for most of the disagreement [22].
2. **Whether humans need to read the code:** no, say StrongDM and OpenAI's harness engineering [87][63]; yes, say the vibe-engineering camp, experienced developers, and open-source projects that ban AI [82][9][97].
3. **Vibe coding in production:** 40% of new enterprise software by 2028 [50], versus "passé" [81], versus banned [97].
4. **Who benefits:** juniors gain most [1][84], yet entry-level employment is falling [20], and experienced developers were slower in one trial [2].
5. **What to measure:** the share of code written by AI [92][93][29], or outcomes [35][31].
6. **Open-source stance:** ban, require disclosure, or accept [39].

## 7. Gaps

- Enterprise platforms (SAP, Salesforce, Databricks, Siemens) are covered only by vendor sources; no independent adoption or outcome data found.
- Other suites (Oracle, Workday, Microsoft Dynamics, ServiceNow) and workflow, BPM, and case-management platforms were not searched.
- Thin: data science beyond pipelines; education outcomes; mobile; small businesses.
- Annual 2026 reports from Stack Overflow and DORA weren't out as of September 30, 2026: Stack Overflow says its results are coming, and DORA's latest is 2025 [25][31]. Stack Overflow's April 2026 pulse survey on agents isn't covered here [25].

## 8. Open questions

- What, if anything, sets enterprise AI coding apart from other kinds of use?
- Is way 4 (an application generated from a description) a way of working of its own, or a variant of way 2 (delegation)?

## Sources

**[R] Research**

1. [Cui, Demirer et al., "The Effects of Generative AI on High-Skilled Work," Management Science](https://pubsonline.informs.org/doi/fpi/10.1287/mnsc.2025.00535)
2. [METR (Becker, Rush, Barnes, Rein), "Measuring the Impact of Early-2025 AI on Experienced Open-Source Developer Productivity," July 2025](https://metr.org/Early_2025_AI_Experienced_OS_Devs_Study-paper.pdf)
3. [METR, "We are Changing our Developer Productivity Experiment Design," February 2026](https://metr.org/blog/2026-02-24-uplift-update/)
4. [Barke, James, Polikarpova, "Grounded Copilot," OOPSLA 2023](https://arxiv.org/pdf/2206.15000)
5. [Hassan et al., "Agentic Software Engineering: Foundational Pillars and a Research Roadmap," 2025](https://arxiv.org/abs/2509.06216v1)
6. ["A Survey of Vibe Coding with Large Language Models," 2025](https://arxiv.org/html/2510.12399v1)
7. [Survey on code generation with LLM-based agents, 2025](https://arxiv.org/abs/2508.00083v2)
8. ["Agentic AI applications in software development: a systematic mapping study," LUT University, 2026](https://lutpub.lut.fi/handle/10024/171646)
9. ["Professional Software Developers Don't Vibe, They Control," 2025](https://arxiv.org/abs/2512.14012)
10. ["AIDev: Studying AI Coding Agents on GitHub," 2026](https://arxiv.org/html/2602.09185v1)
11. ["Migrating Code at Scale with LLMs at Google," FSE 2025](https://arxiv.org/abs/2504.09691v1)
12. ["A survey of generative AI adoption and perceived productivity among scientists who program," 2025](https://arxiv.org/pdf/2512.19644)
13. [ORNL, "Scientific software in the age of vibe coding"](https://impact.ornl.gov/en/publications/scientific-software-in-the-age-of-vibe-coding/)
14. [LLMs for safety-critical automotive code, 2025](https://www.arxiv.org/pdf/2506.23535)
15. [AutoPLC: vendor-aware Structured Text generation](https://arxiv.org/html/2412.02410v2)
16. ["Large Language Model for Verilog Code Generation: Literature Review and the Road Ahead," 2025](https://arxiv.org/html/2512.00020v2)
17. [NVIDIA, Spec2RTL-Agent, 2025](https://research.nvidia.com/publication/2025-06_spec2rtl-agent-automated-hardware-code-generation-complex-specifications-using)
18. [MetaGPT, 2023](https://arxiv.org/html/2308.00352v7)
19. [Learning-oriented AI coding assistants, 2026](https://arxiv.org/abs/2603.22673)
20. [Stanford Digital Economy Lab, "Canaries in the Coal Mine," August 2026 update (via AI Weekly)](https://aiweekly.co/node/10903)
21. [Gu et al., "Challenges and Paths Towards AI for Software Engineering," 2025](https://arxiv.org/abs/2503.22625)
22. [Michels et al., "Vibe Coding: Practice, Performance, Productivity, and Risk," state-of-the-art review, August 2026](https://arxiv.org/abs/2608.20446)
23. [He et al., "AI Writes Faster Than Humans Can Review: A Longitudinal Study of an Enterprise 2x Mandate," July 2026](https://arxiv.org/abs/2607.01904)
24. [Diaz et al., "Spec-Driven Development for Agentic Software Engineering," August 2026](https://arxiv.org/abs/2609.00252)

**[S] Industry surveys, usage data, research programs**

25. [Stack Overflow Developer Survey 2025, AI section](https://survey.stackoverflow.co/2025/ai) · [April 2026 pulse on agents](https://stackoverflow.blog/2026/05/27/agents-on-a-leash-agentic-ai-remains-mostly-monitored-at-work/) · [2026 results announcement, September 30, 2026](https://stackoverflow.blog/2026/09/30/getting-ready-for-2026-results-a-look-back-on-developer-survey-findings/)
26. [JetBrains Research, AI coding agent adoption 2026](https://blog.jetbrains.com/research/2026/08/ai-coding-agent-adoption-2026/)
27. [JetBrains, State of Developer Ecosystem 2025](https://blog.jetbrains.com/research/2025/10/state-of-developer-ecosystem-2025/)
28. [The Pragmatic Engineer, AI tooling 2026](https://newsletter.pragmaticengineer.com/p/ai-tooling-2026)
29. [Sonar, State of Code Developer Survey 2026](https://www.sonarsource.com/company/press-releases/sonar-data-reveals-critical-verification-gap-in-ai-coding/)
30. [SlashData, state of agentic AI adoption in software projects](https://www.slashdata.co/free-industry-reports/the-state-of-agentic-ai-adoption-in-software-projects)
31. [DORA, State of AI-assisted Software Development 2025](https://services.google.com/fh/files/misc/2025_state_of_ai_assisted_software_development.pdf) · [research archive](https://dora.dev/research/)
32. [DORA AI Capabilities Model (Google Cloud)](https://cloud.google.com/blog/products/ai-machine-learning/introducing-doras-inaugural-ai-capabilities-model)
33. [DORA, AI-accessible internal data (updated January 12, 2026)](https://dora.dev/capabilities/ai-accessible-internal-data/)
34. [Faros AI, AI Productivity Paradox Report 2025](https://www.faros.ai/blog/ai-software-engineering)
35. [DX, AI Measurement Framework](https://getdx.com/uploads/ai-measurement-framework.pdf)
36. [RunSafe Security, 2025 AI in Embedded Systems Report](https://www.businesswire.com/news/home/20251209931517/en/RunSafe-Security-Releases-2025-AI-in-Embedded-Systems-Report-Offering-New-Insight-Into-AI-Adoption-and-Security-Gaps)
37. [GDC 2026 State of the Game Industry](https://gdconf.com/article/gdc-2026-state-of-the-game-industry-reveals-impact-of-layoffs-generative-ai-and-more/)
38. [Pulumi, State of Agentic Infrastructure 2026](https://www.pulumi.com/state-of-agentic-infrastructure/)
39. [RedMonk, open-source generative AI policies](https://redmonk.com/kholterhoff/?p=444)
40. [Veracode, 2025 GenAI Code Security Report](https://www.veracode.com/press-release/ai-generated-code-poses-major-security-risks-in-nearly-half-of-all-development-tasks-veracode-research-reveals/) · [2026 follow-up (via SD Times)](https://sdtimes.com/agentic-security/veracode-finds-ai-generated-code-security-has-barely-improved-since-last-year/) · [Spring 2026 update](https://www.veracode.com/blog/spring-2026-genai-code-security/)
41. [CodeRabbit, State of AI vs Human Code Generation](https://www.coderabbit.ai/blog/state-of-ai-vs-human-code-generation-report)
42. [GitClear 2025 code quality research (via LeadDev)](https://leaddev.com/software-quality/how-ai-generated-code-accelerates-technical-debt)
43. [Black Duck OSSRA 2026 (via HeroDevs)](https://www.herodevs.com/blog-posts/68-of-codebases-contain-license-conflicts-and-ai-generated-code-is-making-it-worse)

**[A] Analysts and consultancies**

44. [McKinsey, The State of AI in 2026](https://www.mckinsey.com/~/media/mckinsey/business%20functions/quantumblack/our%20insights/the%20state%20of%20ai/the-state-of-ai-in-2026-on-the-road-to-roi.pdf)
45. [McKinsey Technology Trends Outlook 2026 (via CIO Dive)](https://www.ciodive.com/news/enterprises-bet-coding-agents-despite-ROI/830943/)
46. [Bain, Technology Report 2025: generative AI in software development](https://www.bain.com/insights/from-pilots-to-payoff-generative-ai-in-software-development-technology-report-2025/)
47. [Gartner, enterprise AI coding agents market, May 20, 2026](https://www.gartner.com/en/newsroom/press-releases/2026-05-20-gartner-says-the-market-for-enterprise-ai-coding-agents-is-entering-a-new-phase-of-expansion-and-competitive-realignment)
48. [Gartner 2025 Magic Quadrant for AI Code Assistants (via GitHub)](https://github.blog/ai-and-ml/github-copilot/gartner-positions-github-as-a-leader-in-the-2025-magic-quadrant-for-ai-code-assistants-for-the-second-year-in-a-row/) · [2026 Magic Quadrant for Enterprise AI Coding Agents (via GitHub)](https://github.com/resources/whitepapers/gartner-magic-quadrant-for-enterprise-ai-coding-agents) · [2026 Magic Quadrant coverage (Virtualization Review)](https://virtualizationreview.com/articles/2026/06/05/ai-firms-push-cloud-giants-from-leaders-quadrant-in-gartner-ai-coding-report.aspx)
49. [Gartner Hype Cycle for Artificial Intelligence 2025](https://www.gartner.com/en/articles/hype-cycle-for-artificial-intelligence)
50. [Gartner, vibe coding report (via Legit Security)](https://info.legitsecurity.com/gartner-vibe-coding-report)
51. [Gartner, AI-augmented low-code](https://www.gartner.com/en/documents/7720258)
52. [Forrester, TuringBots in the software development lifecycle](https://www.forrester.com/blogs/ai-and-generative-ai-in-the-software-development-lifecycle)
53. [Menlo Ventures 2025 State of Generative AI in the Enterprise (via SaaStr)](https://www.saastr.com/55-of-all-departmental-ai-spend-is-now-on-coding-and-its-not-slowing-down)
54. [a16z (Appenzeller and Li), "The Trillion Dollar AI Software Development Stack," October 2025](https://a16z.com/the-trillion-dollar-ai-software-development-stack/)

**[G] Government, standards bodies, foundations**

55. [OpenSSF, Security-Focused Guide for AI Code Assistant Instructions](https://best.openssf.org/Security-Focused-Guide-for-AI-Code-Assistant-Instructions)
56. [OWASP, Top 10 for Agentic Applications](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications/)
57. [DARPA, AIxCC results](https://www.darpa.mil/news/2025/aixcc-results)
58. [Linux Foundation, Agentic AI Foundation announcement, December 2025](https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation)

**[V] Vendor-published**

59. [Anthropic Economic Index: AI's impact on software development, April 2025](https://www.anthropic.com/research/impact-software-development)
60. [Anthropic internal study of its engineers (via Fortune)](https://fortune.com/2025/12/02/how-anthropics-safety-first-approach-won-over-big-business-and-how-its-own-engineers-are-using-its-claude-ai)
61. [Anthropic, 2026 Agentic Coding Trends Report](https://resources.anthropic.com/2026-agentic-coding-trends-report)
62. [Anthropic, 2026 State of AI Agents Report](https://resources.anthropic.com/2026-state-of-ai-agents)
63. [OpenAI, "Harness engineering," February 2026](https://openai.com/index/harness-engineering/)
64. [Amazon Q Java upgrades, 4,500 developer-years (via Simon Willison)](https://simonwillison.net/2024/Aug/24/andy-jassy-amazon-ceo/)
65. [Salesforce Agentforce Vibes (via TechCrunch)](https://techcrunch.com/2025/10/01/salesforce-launches-enterprise-vibe-coding-product-agentforce-vibes)
66. [Anthropic, COBOL modernization](https://claude.com/blog/how-ai-helps-break-cost-barrier-cobol-modernization)
67. [Microsoft, AI agents for COBOL migration](https://devblogs.microsoft.com/all-things-azure/how-we-use-ai-agents-for-cobol-migration-and-mainframe-modernization/)
68. [IBM, watsonx Code Assistant for Z](https://www.nasdaq.com/press-release/ibm-unveils-watsonx-generative-ai-capabilities-to-accelerate-mainframe-application)
69. [Google DeepMind, CodeMender (via SiliconANGLE)](https://siliconangle.com/2025/10/06/google-deepmind-unveils-codemender-ai-agent-autonomously-patches-software-vulnerabilities/)
70. [Cadence, ChipStack RTL Generation Agent (via Futurum)](https://futurumgroup.com/?p=95879)
71. [GitHub Octoverse 2025](https://github.blog/news-insights/octoverse/octoverse-a-new-developer-joins-github-every-second-as-ai-leads-typescript-to-1/)
72. [GitHub, About Copilot cloud agent](https://docs.github.com/en/copilot/concepts/agents/cloud-agent/about-cloud-agent)
73. [SAP, Joule for Developers and ABAP AI capabilities, July 2025](https://news.sap.com/2025/07/joule-abap-transform-developer-experience/)
74. [SAP Business AI for IT and developers (Integration Suite via Joule)](https://www.sap.com/products/technology-platform/ai.html)
75. [Databricks, Genie Code for pipeline development](https://docs.databricks.com/aws/en/ldp/de-agent)
76. [AWS Transform custom](https://docs.aws.amazon.com/transform/latest/userguide/custom.html)
77. [Siemens, Eigen Engineering Agent for TIA Portal](https://www.siemens.com/lv-lv/products/tia-portal/eigen-engineering-agent/)
78. [AWS Transform for mainframe (documentation)](https://docs.aws.amazon.com/transform/latest/userguide/transform-app-mainframe.html)
79. [Lovable, "$100M ARR & Lovable Agent," July 2025](https://lovable.dev/blog/agent)

**[P] Practitioners and press**

80. [Karpathy, "Software Is Changing (Again)" (via Latent Space)](https://www.latent.space/p/s3)
81. [Karpathy, "agentic engineering" (via The New Stack)](https://thenewstack.io/vibe-coding-is-passe/)
82. [Willison, "Not all AI-assisted programming is vibe coding (but vibe coding rocks)," March 2025](https://simonwillison.net/2025/Mar/19/vibe-coding/) · ["Vibe engineering," October 2025](https://simonwillison.net/2025/Oct/7/vibe-engineering/)
83. [Shapiro, the five levels (via Simon Willison)](https://simonwillison.net/2026/Jan/28/the-five-levels/)
84. [Yegge, "Revenge of the junior developer"](https://sourcegraph.com/blog/revenge-of-the-junior-developer)
85. [Böckeler, spec-driven development tools (martinfowler.com)](https://www.martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html)
86. [Thoughtworks Technology Radar](https://www.thoughtworks.com/radar)
87. [StrongDM software factory (via Simon Willison)](https://simonwillison.net/2026/Feb/7/software-factory/)
88. [Shopify "reflexive AI usage" memo (via Digital Commerce 360)](https://www.digitalcommerce360.com/2025/04/08/internal-memo-shopify-ceo-declares-ai-non-optional/)
89. [Coinbase AI coding mandate (via Fortune)](https://fortune.com/2025/08/25/coinbase-ceo-brian-armstrong-ai-coding-assistants-mandate-tech)
90. [Coinbase, about 40% of daily code AI-generated (via The Block)](https://www.theblock.co/post/369460/brian-armstrong-coinbase-ai)
91. [Y Combinator Winter 2025 batch (via NBC)](https://www.nbcnewyork.com/news/business/money-report/y-combinator-startups-are-fastest-growing-most-profitable-in-fund-history-because-of-ai/6188199/)
92. [Microsoft, 20–30% of code AI-generated (via TechRadar)](https://www.techradar.com/pro/a-shockingly-high-amount-of-microsoft-code-is-now-written-by-ai-it-admits)
93. [Google, most new code AI-generated (via MobileSyrup)](https://mobilesyrup.com/2026/04/24/ai-is-generating-most-of-googles-code/)
94. [Goldman Sachs and Devin (via Quartz)](https://qz.com/goldman-sachs-ai-coder-devin-cognition)
95. [Replit production database incident (via eWeek)](https://www.eweek.com/news/replit-ai-coding-assistant-failure/)
96. [Vibe coding tools for non-developers (Designlab)](https://designlab.com/blog/best-vibe-coding-tools)
97. [Godot AI contribution policy (via Let's Data Science)](https://letsdatascience.com/news/godot-tightens-rules-on-ai-contributed-code-8f3a9630)
98. [GitHub weighs pull-request controls (via OpenSourceForU)](https://www.opensourceforu.com/2026/02/github-weighs-pull-request-kill-switch-as-ai-slop-floods-open-source/)
99. [IBM share drop after Anthropic's COBOL post (via Sherwood)](https://sherwood.news/markets/ibm-sinks-as-anthropic-positions-claude-code-as-the-ideal-tool-for-code/)
100. [Siemens Engineering Copilot TIA at SPS 2025 (via Automation World)](https://automationworld.com/factory/digital-transformation/news/55332816/siemens-ag-siemens-unveils-generative-ai-copilot-for-autonomous-engineering-at-sps-2025)
101. [amux, "The AI Coding Tools Landscape (2026)" (a practitioner map)](https://amux.io/guides/ai-coding-tools-landscape-2026/)
102. [Tech Lead Journal #255, Brian Madison on the BMad Method](https://techleadjournal.dev/episodes/255/)
103. [Y Combinator, "How Replit Went From $10M to $100M ARR In Just 9 Months" (interview with CEO Amjad Masad)](https://www.youtube.com/watch?v=kOyIjt6FUrw)
