# 33. Developing C1 Software with AI: Today and the Path Ahead

*Originally published on LinkedIn: <https://lnkd.in/p/gE9xvGSd>*

## A different question: How is AI changing development?

Post 32 said this post would take its three categories through real examples. The examples turned out to be familiar, so this post goes straight to a different question: how is AI changing the way C1 software, software that runs on code, is developed, today and on the path ahead? C2 and C3 follow in Posts 34 and 35. C1 comes first because its development methods predate AI, which gives a baseline for what AI changes. The categories themselves won't change: they sort software by the stack it runs on, and Post 32's two fundamental facts keep the stacks separate.

The business team wants software to use in its business. It asks the development team to build it, and provides the requirements. The requirements do two jobs: they tell the development team what to build, and they give the business team its criteria for accepting the result. The development team handles the work between requirements and acceptance. In practice, the teams work through repeated iterations, with feedback flowing both ways. Within one logical iteration, this post looks inside the development team: how human developers and AI share the work, and how development methods change as AI's participation deepens. The resulting software remains C1 because code still carries its business function.

Two terms keep that boundary clear. Requirements come from the business. Specs belong to the development team, which writes them from the requirements.

The spec is the first link in a chain that turns requirements into software. From the spec come a design, sometimes at several levels, and a test plan. From the design comes the code, and from the test plan the test code. Then the code is checked: by automated tests, by people testing it, and by review. The links produce artifacts; the checks are actions. Sharing the work between developers and AI means deciding who handles each link and each check.

Post 32 said the series would turn to "an AI-native SDLC for C1." In this post, AI-native describes how methods are designed: for AI rather than for people. It does not measure how much of the work AI does. An AI-native SDLC is one whose methods, languages, and toolchains are designed for AI, and the steps toward it are the path toward an AI-native SDLC.

The post starts with today's practice, then follows three steps along that path. Each step answers the same three questions: what AI takes on, how developers' work changes, and what engineering work makes the change possible. The steps are my reading of current practices and experiments, not a sequence every team must follow, and they can overlap.

## Today: AI inside methods designed for people

Nearly all professional developers now use AI, and agents are where use is growing. In JetBrains' 2026 survey of more than 15,000 professional developers, 90% were using AI coding agents at work at least weekly, and 68% daily [1].

Post 31 sorted how they work into four ways. In interactive assistance, the most common, a developer works with AI in a session and reviews along the way. In delegation, a developer hands a task to an agent, which returns a pull request for review. In validated transformation, a change such as a migration or an upgrade is defined once, checked, and applied across many repositories. The fourth way, an application generated from a description, often skips the development team altogether, so it falls outside this post.

In the three ways that stay inside the development team, AI works inside methods designed for people. Developers prompt, read, and review, and the code is written in languages designed for people to read. In terms of the chain, AI handles single links, mostly turning designs into code and test plans into test code, or carries a delegated task through several links and returns it for review. Developers own the rest of the chain and the checks.

The gains are real, and so is the pressure they create. At a mid-sized company that set out to double merged pull requests per engineer, authored pull requests per active developer reached 2.09 times the pre-mandate baseline, and each reviewer's load roughly doubled [2]. In Faros AI's telemetry from more than 10,000 developers, those on teams with high AI adoption completed 21% more tasks and merged 98% more pull requests, but pull-request review time rose 91%, and Faros "observed no significant correlation between AI adoption and improvements at the company level" [3]. Bain finds that writing and testing code "only accounts for about 25% to 35% of the time from initial idea to product launch." Teams using AI assistants see gains of 10% to 15%, while "some companies report 25% to 30%" by pairing generative AI with end-to-end process transformation [4].

Where the gains held, the methods around them were ready. Among the wins executives cite, McKinsey's David Pralong points to software teams that already had "testing, continuous integration, and code review practices" that let them inspect and correct new work [5]. Even code completion, before agents, raised completed tasks by about 26% in randomized trials with 4,867 developers [6].

Context matters too. DORA reports that giving AI tools direct access to internal data serves as "a statistically significant multiplier for individual effectiveness and code quality," often through what it calls context engineering [7].

The pressure lands on review. Post 32 put C1's core problem in one line: "code arrives faster than anyone can own it." At the company with the mandate, the share of pull requests with an AI review rose from about 19% to about 84%, while the share with a human-written review comment fell from about 39% to about 21%. Merge and revert rates, which the authors treat as coarse quality proxies, held steady [2]. The division of review work is already changing. The rest of this post follows where that change can lead, and the engineering work that makes each step possible.

**Today, AI works inside methods designed for people, and review is where those methods strain.**

## Developers write the specs; AI writes the code

- **What AI takes on:** the links after the spec. It drafts the design and the test plan, writes the code and the test code, and checks the code against the tests.
- **How developers' work changes:** from writing code to writing specs. Developers translate the business's requirements, including its acceptance criteria, into specs, then review what the links produce: the design, the test plan, and samples of the code.
- **The engineering work that enables it:** expressing intent precisely, maintaining specs across iterations, and supplying context.

Spec-driven tools build this step into the workflow. Birgitta Böckeler compares three of them: Kiro, Spec Kit, and Tessl [8]. A Kiro spec consists of three files: requirements.md, design.md, and tasks.md. They follow the chain: a spec, a design, and the tasks that produce the code. The first is named after requirements, but in this post's terms it is a spec: the developers' translation of what the business asked for.

Böckeler also names three levels of commitment. In spec-first, "a well thought-out spec is written first, and then used in the AI-assisted development workflow for the task at hand." In spec-anchored, "the spec is kept even after the task is complete, to continue using it for evolution and maintenance of the respective feature." In spec-as-source, "the spec is the main source file over time, and only the spec is edited by the human, the human never touches the code" [8]. The higher levels keep specs across iterations, so specs need the same care as code.

AWS describes a broader method, the AI-Driven Development Life Cycle, in which "AI creates a plan, asks clarifying questions to seek context, and implements solutions only after receiving human validation" [9]. The clarifying questions point to the third kind of enabling work: the context that DORA found to be a multiplier [7].

This step is still designed for people, and the evidence is early. Diaz et al. draw "predominantly on gray literature," because "peer-reviewed evidence and a shared academic-industrial vocabulary are not yet established" [10]. Böckeler found the review burden moving rather than shrinking: "I'd rather review code than all these markdown files." In her trials, the agent did not always follow the spec [8]. A spec says what the code must do. It does not guarantee that the code does it. That gap is what the next step works on.

**The developer's work moves from writing code to writing what the code must do.**

## Developers build the checks; machines review chosen code

- **What AI takes on:** the checks people did: testing the software as a user would and reviewing the code, up to approving the code a team chooses. Formal proofs add a check people could rarely afford.
- **How developers' work changes:** from reading code to building the checks, and choosing which code machines may approve.
- **The engineering work that enables it:** turning acceptance criteria into executable checks, enforcing architecture with tools, giving agents a way to run and observe the software, and showing that quality holds for the chosen code.

Tools already allow it. Since September 1, 2026, GitHub has let Copilot "submit an approval that counts toward the repository's required-approvals rule." The feature is off by default and in public preview, and admins "choose which file paths Copilot is allowed to approve" [11]. Choosing those paths is engineering work in itself: deciding which code machines may approve.

OpenAI's harness engineering team went further. Over five months, a team that grew from three engineers to seven built an internal product with no hand-written code, merging roughly 1,500 pull requests into a repository of about a million lines, counting application logic, infrastructure, tooling, and documentation. "Humans may review pull requests, but aren't required to. Over time, we've pushed almost all review effort towards being handled agent-to-agent" [12]. The team describes its job this way: "The primary job of our engineering team became enabling the agents to do useful work." That work included:

- a short AGENTS.md that gives the agent "a map, not a 1,000-page instruction manual," pointing to a structured docs directory treated as the system of record;
- architecture rules enforced "mechanically via custom linters";
- an app that boots for each change, with logs, metrics, and traces the agent can read;
- "golden principles" written into the repository, with background agent tasks that find deviations and open refactoring pull requests.

The decisions at the boundary stayed with people: "We prioritize work, translate user feedback into acceptance criteria, and validate outcomes" [12].

StrongDM's software factory removes human review by rule: "Code must not be written by humans" and "Code must not be reviewed by humans" [13]. In place of review, the factory uses scenarios: end-to-end user stories, often "stored outside the codebase (similar to a 'holdout' set in model training)." The scenarios run against a "Digital Twin Universe," behavioral copies of services such as Okta, Jira, and Slack, built by "replicating their APIs, edge cases, and observable behaviors." Success becomes a probability: "of all the observed trajectories through all the scenarios, what fraction of them likely satisfy the user?" [13]. StrongDM builds software that contains agents, so the technique, not its products, is the example here. Its measure covers simulated scenarios, not outcomes in a deployed business.

Formal verification is the strictest check. Developers write formal specs, AI writes the proofs, and a proof checker establishes that the code conforms to the spec. Martin Kleppmann predicts that "AI will bring formal verification, which for decades has been a bit of a fringe pursuit, into the software engineering mainstream." Hallucinated proofs don't matter, "because the proof checker will reject any invalid proof and force the AI agent to retry" [14]. Early results point the same way: a system called Aria proved "all 4,257 lemmas of the four core modules" of the Iris verification framework, and 217 lemmas verifying Rust standard libraries built on it, with "no failures and no Coq expert intervention" [15]. Routine verification of business applications is still ahead. And the hard part moves to the spec: "The challenge will move to correctly defining the specification," Kleppmann writes, and "reading and writing such formal specifications still requires expertise and careful thought" [14].

Quality belongs in this step, because establishing it is part of the engineering work. General parity with human-written code has not been established. The aggregate findings come from vendors. CodeRabbit counted about 1.7 times as many issues in AI-co-authored pull requests as in human-only ones, across 470 open-source pull requests [16]. Veracode found that when models complete code without security-specific prompting or guidance, 45% of tasks introduce a known security flaw, and that security pass rates are "virtually identical to where they stood two years ago" [17]. Neither finding rules out parity for chosen code, which is why the checks are chosen code by code. Shipping without human review does not, by itself, show parity.

OpenAI's team states its limits too. Agents copy the patterns already in the repository, "even uneven or suboptimal ones," which over time "inevitably leads to drift." And the approach "should not be assumed to generalize without similar investment—at least, not yet" [12].

**The developer's work moves from reading code to building what checks it.**

## Developers steer; AI works in languages designed for it

- **What AI takes on:** artifacts designed for it. Code becomes a structure that AI edits directly, such as a semantic graph, and people read a projection of it.
- **How developers' work changes:** developers steer. They decide what to build and how it is checked, and they read the code when it helps.
- **The engineering work that enables it:** building and maintaining languages and toolchains that check every edit, and keeping a readable projection for people.

In this step, the programming environment itself changes. Languages, toolchains, and practices are designed for AI rather than for people. This is where the trend points: an AI-native SDLC.

Zerolang, from Vercel Labs, is an experimental language in which "the semantic graph is the program database" [18]. "Agents query the graph, submit checked edits, and prove the result," and the edits "target semantic structure instead of line ranges." The compiler validates each edit's structure and types; whether an edit does what was asked is still a separate check. Text files remain as projections: "Humans can read it, review it, and occasionally edit it." The project is candid about its state: "Zerolang is experimental. Expect breaking changes, rough edges, and security issues" [18].

Code in a language designed for AI is still C1: a compiler turns it into something the CPU runs.

An AI-native SDLC need not cover a whole codebase. Quality is established for particular code, so one codebase can sit at several steps at once: methods designed for AI for some code, specs for some, and human review for the rest.

**The environment is built for AI, and developers build and steer the environment.**

## The trend, and the work behind it

Across the steps, AI takes on more of the chain: first single links, then every link after the spec, then the checks, and finally artifacts designed for it. Development methods move with it, from methods designed for people toward methods designed for AI. Engineering work makes each step workable: translating requirements into specs, supplying context, building checks against acceptance criteria, and building the toolchains. Specs, checks, harnesses, and toolchains persist across iterations. Teams build them once and maintain them, rather than rebuilding them every iteration.

Post 30 named four responsibilities for our side: requirements, context, acceptance criteria, and operating outcomes. Within one iteration, the requirements come from the business and double as its acceptance criteria, and operating outcomes come after acceptance. The development team's share is to turn requirements into specs and acceptance criteria into checks, and to supply context. All four stay connected to the business at every step.

Faster iterations will also change how the two teams work together. AWS's AI-DLC proposes replacing sprints with shorter cycles it calls "bolts" [9]. That belongs to a later post on how the teams iterate.

My reading: in C1, how far AI's involvement goes depends less on AI's ability to write code than on our ability to say what the code must do and to check that it does. C1 runs on specification, so specs, tests, scenarios, and proofs can carry the checking. C2 cannot lean on that. Its business work runs on judgment, so it is checked by evaluation (Post 32). Post 34 takes C2.

**How the software is built changes; its classification as C1 does not.**

---

## References

[1] Mikhail Bogdanov, *AI Coding Agents: Adoption Trends*, JetBrains Research, August 2026. <https://blog.jetbrains.com/research/2026/08/ai-coding-agent-adoption-2026/>. JetBrains' Developer Ecosystem Survey 2026: more than 15,000 professional developers, May–July 2026.

[2] Hao He, Shyam Agarwal, Yegor Denisov-Blanch, Pavel Azaletskiy, Sanmi Koyejo, and Bogdan Vasilescu, *AI Writes Faster Than Humans Can Review: A Longitudinal Study of an Enterprise 2x Mandate*, arXiv:2607.01904, July 2, 2026. <https://arxiv.org/abs/2607.01904>. 802 developers and 196,212 pull requests at one mid-sized company, January 2024 to April 2026; the review shares are in section IV-C.

[3] Faros Research, *The AI Productivity Paradox Report 2025*, Faros AI, July 23, 2025. <https://www.faros.ai/blog/ai-software-engineering>. Telemetry from more than 10,000 developers on 1,255 teams; Faros AI sells engineering analytics. The date couldn't be verified.

[4] Purna Doddapaneni, Bill Radzevych, Steven Breeden, Bharat Bansal, and Tanvee Rao, *From Pilots to Payoff: Generative AI in Software Development*, Bain Technology Report 2025, September 23, 2025. <https://www.bain.com/insights/from-pilots-to-payoff-generative-ai-in-software-development-technology-report-2025/>

[5] David Pralong, *McKinsey senior partner: Why AI's easiest wins are misleading CEOs*, Fortune, October 1, 2026. <https://fortune.com/2026/10/01/mckinsey-senior-partner-why-ais-easiest-wins-are-misleading-ceos/>

[6] Kevin Zheyuan Cui, Mert Demirer, Sonia Jaffe, Leon Musolff, Sida Peng, and Tobias Salz, *The Effects of Generative AI on High-Skilled Work: Evidence from Three Field Experiments with Software Developers*, Management Science, published online February 27, 2026. <https://doi.org/10.1287/mnsc.2025.00535>. Randomized trials at Microsoft, Accenture, and an anonymous Fortune 100 company, with code completion, before coding agents; the 26.08% increase has a standard error of 10.3 percentage points.

[7] DORA, *Capabilities: AI-accessible internal data*, updated January 12, 2026. <https://dora.dev/capabilities/ai-accessible-internal-data/>

[8] Birgitta Böckeler, *Understanding Spec-Driven-Development: Kiro, spec-kit, and Tessl*, martinfowler.com, October 15, 2025. <https://www.martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html>

[9] Raja SP, *AI-Driven Development Life Cycle: Reimagining Software Engineering*, AWS DevOps Blog, July 31, 2025. <https://aws.amazon.com/blogs/devops/ai-driven-development-life-cycle/>. A methodology from a vendor of AI development tools.

[10] Jessica Diaz, Joaquin Gayoso, Andrea Cimminio, and Jorge Perez, *Spec-Driven Development for Agentic Software Engineering: Harnessing Human-Agent Teamwork*, arXiv:2609.00252, August 31, 2026. <https://arxiv.org/abs/2609.00252>. A conceptual analysis.

[11] GitHub, *Copilot code review can now approve pull requests*, GitHub Changelog, September 1, 2026. <https://github.blog/changelog/2026-09-01-copilot-code-review-can-now-approve-pull-requests>. Public preview; Copilot does not approve pull requests unless an admin enables it.

[12] Ryan Lopopolo, *Harness engineering: leveraging Codex in an agent-first world*, OpenAI, February 11, 2026. <https://openai.com/index/harness-engineering/>. OpenAI's own account of building an internal product; the first commit was in late August 2025.

[13] Justin McCarthy, *Software Factories And The Agentic Moment*, StrongDM, February 6, 2026. <https://factory.strongdm.ai/>. StrongDM's own account of its practice.

[14] Martin Kleppmann, *Prediction: AI will make formal verification go mainstream*, December 8, 2025. <https://martin.kleppmann.com/2025/12/08/ai-formal-verification.html>

[15] Shuangxiang Kan, Shuanglong Kan, and Sebastian Ertel, *Harnessing Code Agents for Automatic Software Verification*, arXiv:2607.06341, July 7, 2026. <https://arxiv.org/abs/2607.06341>. A preprint. Iris is a separation-logic framework built in Coq. The experiments reconstruct proofs for existing lemma statements after removing the original proofs; they don't derive specifications or verify new business applications.

[16] David Loker, *Our new report: AI code creates 1.7x more problems*, CodeRabbit blog, December 17, 2025. <https://www.coderabbit.ai/blog/state-of-ai-vs-human-code-generation-report>. The blog's account of CodeRabbit's State of AI vs Human Code Generation Report: 10.83 issues per AI-co-authored pull request against 6.45 per human-only one. Of the 470 pull requests, 320 were inferred to be AI-co-authored from co-authorship signals, and 150 were presumed human-only. CodeRabbit sells AI code review.

[17] Felix Brombacher, *Spring 2026 GenAI Code Security Update: Despite Claims, AI Models Are Still Failing Security*, Veracode, March 24, 2026. <https://www.veracode.com/blog/spring-2026-genai-code-security/>. Function-completion tasks across four vulnerability classes, without security-specific prompting or guidance: more than 150 models on 80 tasks in Java, JavaScript, C#, and Python. Veracode sells application security testing. The date and full title couldn't be verified.

[18] Vercel Labs, *Zerolang*, GitHub repository, accessed October 2026. <https://github.com/vercel-labs/zerolang>

## Earlier in this series

All posts are archived in the GitHub repository: <https://github.com/young-rex/notes-on-agentic-ai>

- Post 32: *AI Coding Has Two Sides. Ours Starts with What Runs the Software.* <https://lnkd.in/p/gacN7SgW>
- Post 31: *AI Coding Has Two Sides. Ours Is Too Big for One Post.* <https://lnkd.in/p/gpS8XrPn>
- Post 30: *AI Coding Has Two Sides. First, Theirs.* <https://lnkd.in/p/gyg_buW9>
