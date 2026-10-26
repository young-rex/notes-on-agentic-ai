# 37. AI-Native SDLC: The Landscape

*Originally published on LinkedIn: <https://lnkd.in/p/gkDevySp>*

## Many proposals for AI in the SDLC

Post 36 followed a retailer's refund change through seven duties: settle the rule, find what it touches, decide what counts as correct, change the code and the tests, review and release, operate and observe, and feed back. Different people may hold those duties, or one person several, and each sees part of the path. Post 36 also separated responsibilities, methods, and tools, and asked three questions about any arrangement: is it described, is it implemented, and has it been shown to work, where, and for what?

From here on, people and AI agents share that work. For each activity on the path, who performs it, who decides, and who answers for the result can differ. In this series, whoever or whatever performs an activity is an actor: a person, automation such as a CI server, or an AI agent. Performing an activity doesn't by itself give an actor the authority to decide. An agent acts within the permissions it is given, and accountability stays with people. Post 36 ended by asking what changes when "a participant can perform substantial duties but sees and acts only through the context, tools, and permissions it is given." That participant is the new actor.

Proposals to rearrange that path around AI arrive under many names. AWS calls its method the AI-Driven Development Life Cycle [1]. Anthropic publishes an AI-native SDLC playbook [2]. Thoughtworks describes AI-first software delivery [3], GitHub spec-driven development [4], and Microsoft agentic DevOps [5]. Kiro's homepage invites you to "Move beyond AI coding to agentic engineering" [6]. Sonar has an Agent Centric Development Cycle [7], and Google's Conductor offers context-driven development [8].

Each proposal's label is its publisher's choice, and I use it as is. Post 33 used AI-native for methods designed for AI rather than for people, not for how much of the work AI does. That is this series' lens, not an industry definition, and later posts apply it. By that lens, adding an AI actor changes who does the work; it doesn't by itself make the arrangement AI-native. Calling these "AI-native proposals" names the set; it isn't a verdict on each one.

The proposals differ, and not only in name. Some are methods with no software of their own, some are products with a workflow built in, and some are both under one name.

**A label names a proposal; it doesn't say what the proposal arranges.**

## The landscape at a glance

This post surveys eighteen of these proposals. It maps what each one documents: what it arranges, how far along Post 36's path it reaches, which mechanisms it uses, and what evidence exists. It doesn't judge which works best. I read the proposals' documentation, release records, and workflow files as of October 2026; I didn't install or run them. The set is a reviewed selection with stated rules, not a census or a ranking; the appendix explains how I selected and classified it.

The eighteen fall into four groups by where the method lives and what carries it:

- **Methods only.** The publisher prescribes an arrangement and leaves implementation to the adopting organization, with general coding agents.
- **Methods with their own assets.** The method ships workflow files, skills, commands, or a command-line tool that carry it into an agent's work.
- **Methods built into products.** The method is a workflow built into an agentic development product.
- **Methods delivered through the publisher's platform.** The arrangement is carried out through the publisher's own products and services.

This survey reads every proposal through six lenses, which later posts inherit:

- **Responsibilities:** which duties does AI perform, and which decisions stay with people?
- **Context:** what does the agent see, how is that selected, and which artifact is the source of truth?
- **Coordination:** who or what selects the next action and passes the work on?
- **Checks and authority:** what produces evidence, what enforces a condition, and who accepts the result?
- **Reach:** which duties does the arrangement cover, and what does it leave to the adopting organization?
- **Evidence:** is it described, is it implemented, and has it been shown to work, where, and for what?

The tables boil the first four down to a main mechanism. The sections after them cover reach, checks and authority, and evidence, in that order.

### Methods only

| Proposal | Publisher | Main mechanism | Documented reach | Evidence |
|---|---|---|---|---|
| AI-native SDLC playbook [2] | Anthropic | Versioned artifacts from intent.md on; hooks as approval gates; production signals written back as new intent | All seven, as guidance and recipes | Playbook and recipes; reported practice |
| Building an AI-Native Engineering Team [9] | OpenAI | Delegate, review, own | Through release; diagnosis and fixes in maintenance | Guide; publisher's examples |
| The New SDLC With Vibe Coding [10] | Google | Harness and context design; tests and evals define "correct" | From the coauthor's account: through testing; agent-product deployment example; monitoring scope interpreted in [10] | Whitepaper; coauthor's account |
| AI-Native SAFe [11] | Scaled Agile | Enterprise operating model; an innovation pipeline from discovery to release | Through release; telemetry feeds learning | Framework and training |

### Methods with their own assets

| Proposal | Publisher | Main mechanism | Documented reach | Evidence |
|---|---|---|---|---|
| AI-DLC [1] | AWS | AI proposes and decomposes; people approve at gates | All seven in the method; operation conditional in the workflows | Method, workflow files; one customer account [12] |
| BMAD Method [13] | Open-source project | Roles played by agents; one owner per document; five decision points, two optionally unattended | Through code review; retrospectives | Workflow files |
| Superpowers [14] | Open-source project | Skills for design, plans, test-driven development, and review | Through code review and branch completion | Skills |
| Spec Kit [4] | GitHub | Specs, plans, and tasks; checks from a read-only report to a prompted gate | Through changing the code | Toolkit; census footprint |
| OpenSpec [15] | Open-source project | Specification deltas; coherence review; a verification that reports | Through changing the code | CLI and skills; census footprint |
| Conductor [8] | Google | Project context; a spec and plan per track, approved before code | Through changing the code | Extension; census footprint |
| Spec Kitty [16] | Open-source project | Work packages in lanes; isolated worktrees; review, accept, and merge gates | Through review and merge; retrospectives | CLI; small census footprint |
| GSD Core [17] | Open-source project | A five-step loop per phase; a fresh context per executor; acceptance checkpoints | Through a pull request per phase | Commands; predecessor's census footprint |

### Methods built into products

| Proposal | Publisher | Main mechanism | Documented reach | Evidence |
|---|---|---|---|---|
| Kiro [6] | AWS | Specs with or without approval between artifacts; steering files; property tests; hooks and permissions | Through changing the code | Product docs; one customer account [12]; census footprint |
| Duo Agent Platform [18] | GitLab | Software Development Flow: context gathering, a plan for review, changes staged for the person's acceptance | From finding what it touches through review of changes | Product docs |

### Methods delivered through the publisher's platform

| Proposal | Publisher | Main mechanism | Documented reach | Evidence |
|---|---|---|---|---|
| Agentic DevOps [5] | Microsoft | Agents on issues; review before shipping; incident work returned as issues | From changing the code through feedback | Announcement; product docs |
| AI SDLC transformation playbook [19] | Atlassian | Enterprise context through a graph of work; coordination; measurement | Planning through review, in part; incidents feed planning | Playbook; one component study [20] |
| AI-first software delivery [3] | Thoughtworks | Living specification; coordinated agents; traceability | All seven, plus modernization | Vendor-reported client outcomes |
| AC/DC [7] | Sonar | Agent guidance; a quality gate before merge; remediation agents | From changing the code through merge; code maintenance | Docs and launch materials |

Reach names the duties a documented workflow covers, starting from settling the rule unless noted. A gap means the sources I reviewed don't document a procedure for that duty, not that the method or its tools cannot support the work. Evidence names what exists: documentation and assets, a customer account, a component study, or a census footprint. A census footprint means the proposal's files turned up in public repositories in a July 2026 census of spec-driven development [21]. GSD Core succeeds GSD, whose repository was archived in June; the census's count describes GSD's use, not GSD Core's.

In every group, the arrangement assigns activities to agents; the groups differ in where the method lives and what carries it. That difference matters because a tool can host a method it doesn't implement. AI-DLC's workflow files run natively in Kiro, Claude Code, Cursor, and other agent environments [1]. Kiro also has its own spec workflow, with its own modes and approvals. Running AI-DLC in Kiro doesn't make Kiro's specs an implementation of AI-DLC, and AI-DLC's coverage of operation doesn't transfer to them. Running Scrum in Jira doesn't show that a team follows Scrum either.

**Hosting a method doesn't make a tool an implementation of it, and a method's coverage doesn't transfer to the tool that hosts it.**

## How far along the path

Development is the clearest common ground. Nearly every proposal documents work from settling or receiving intent to changing the code. Later starting points include GitLab's Software Development Flow, which begins by finding what a change touches [18]; Sonar, which applies its cycle "once development teams have determined the scope and intent of their code" [7]; and Agentic DevOps, which begins with issues assigned to agents [5].

Past merge, reach depends on the kind of proposal. Operation and feedback come mostly from playbooks and platforms. Anthropic's playbook turns a breached control band in production into a new intent.md [2]. Microsoft's SRE Agent investigates incidents and logs remediation as GitHub issues [5]. Atlassian's agents trace an incident's impact and "feed related work items back into planning" [19]. Scaled Agile's pipeline turns release telemetry into learning [11], and Thoughtworks' operations environment updates its living specification when it detects change [3]. Among the eight methods that ship their own assets, only AI-DLC documents operation, and its workflow files make all seven Operation stages conditional: "mvp, poc and classic skip Operation entirely" [1]. Sonar's maintenance loop works on code already in the repository, not on the running service [7].

I found no procedure for retiring a service in the sources I reviewed, though the refund service will one day be replaced. Few proposals reach as far as Post 36's dashboards and support tickets.

Coverage isn't enforcement. A duty counts as covered when a documented workflow addresses it, whether through a checklist, a report, or a gate. What can stop the work is a separate question.

**Development is common ground; operational reach depends on the kind of proposal.**

## Who decides, as documented

Post 36 noted that some duties are decisions, and deciding takes authority as well as knowledge. The proposals place those decisions in four kinds of mechanism, often blurred together as "human in the loop." Here are documented examples of each, without judging how well they work:

- **Instructions** say what the agent should do. Superpowers' README says the agent "checks for relevant skills before any task" and calls them "Mandatory workflows, not suggestions"; its review step reads "Critical issues block progress" [14]. Both are instructions the agent reads; the README doesn't describe a configured gate. A gate can be an instruction too: when checklist items are unchecked, Spec Kit's Implement command tells the agent to "**STOP** and ask" whether to proceed, and to halt if the answer is no [4]. It holds only as long as the agent follows the command's template.
- **Review points** ask a person to approve before the next step. Conductor: "Once you approve the plan, run `/conductor:implement`" [8]. In Kiro, the route decides whether review points exist at all. A Feature Spec asks for approval of the requirements, the design, and the tasks in turn; Quick Spec, a mode of the same workflow, generates all three "with no approval gates between phases," for people to review afterward [6].
- **Executable checks and permissions** enforce rules outside the agent, rather than relying on it to follow instructions. In Kiro's IDE and command line, a hook command that fails before a tool call blocks the call, and permission rules deny, ask, or allow tool actions, with deny taking priority [6]. Microsoft's SRE Agent has run modes. In Review mode, an administrator must approve its Azure infrastructure operations before they run; other actions, such as sending email or posting to Teams, go ahead based on the agent's reasoning and the response plan, so they need separate controls. In Autonomous mode, it acts without waiting [5].
- **Business acceptance** decides that the result is right for the business. A review, merge, or epic-acceptance verdict doesn't by itself mean that someone with business authority has accepted the result. In Spec Kitty, "reviewers accept, reject, or merge with an audit trail" [16]. BMAD's retrospective judges whether an epic met "its own acceptance criteria" and returns "accepted," "accepted-with-open-items," or "rejected," a verdict that "a human can still override" [13]. Spec Kit's checklists record a reviewer's judgment of requirement quality, and its quickstart warns that "Checked custom items do not mean implementation work is complete" [4].

One proposal can combine several mechanisms. Spec Kit's commands include a read-only report, reviewer-owned checklists, a prompted gate, and a check that appends tasks, all carried out as the agent follows command templates. The documentation shows where each proposal places a decision. Whether that decision holds is another matter. Does an approver have the authority the approval implies? Can an agent write the record its own gate reads? The answers depend on assumptions that the next post examines.

**Documentation shows where a proposal places decisions; whether they hold is the next post's question.**

## What has been shown

Post 36's three questions sort the evidence. All eighteen are described. Some proposals supply open workflow files or commercial products; others provide recipes and guidance that the adopting organization must turn into an implementation. For "shown to work," you need to know who produced the evidence, what it covers, and what outcome it looked at. Otherwise, a result for one component can read like a verdict on a whole method.

- **Publisher documentation and assets** show what is described and claimed to be implemented, not that it works in a project.
- **A customer account.** On AWS's Architecture Blog, Dynata reports adopting AI-DLC and Kiro together with preparation and process changes [12]. Its headline figure, about a 70 percent reduction in development-phase time, is Dynata's own estimate. Two of the four authors are AWS staff, some teams ran AI-DLC in Cursor with Claude rather than in Kiro, and there is no controlled comparison. It is one organization's report, and it can't isolate Kiro's effect.
- **A publisher-measured component study.** Atlassian compared pull-request throughput across 3,400 repositories from 2,500 customers, matching Rovo Dev adopters to similar repositories that hadn't adopted it, and surveyed more than 6,200 of its own developers [20]. The study is about component workflows and appeared four months before Atlassian's playbook, so it can't validate the playbook.
- **Independent research on use.** The census found Kiro, Spec Kit, and OpenSpec files in thousands of public repositories each [21]. That is evidence of use within its sample, not of results or market share.
- **A comparative preprint.** *From Prompt to Process* rates prescribed process from documentation [22]. Its author created one of the six frameworks it compares, and one person assigned the scores. It compares what is prescribed, not what happened.

Compare like with like. Putting an AI proposal's best-case description next to the documented problems of established practice would tilt the comparison. And using a coding assistant isn't the same as adopting a named lifecycle.

**In the sources I reviewed, no complete arrangement has been independently shown to work.**

## What the next post examines

The landscape shows where each proposal places context, coordination, checks, and authority. It doesn't show what happens when those mechanisms meet a real business change. The next post follows the refund change through four arrangements, one question each:

- **Who decides when work is split and gated?** AWS AI-DLC, with Kiro's own spec workflow as the contrast.
- **What does a role mean when an agent plays it?** BMAD.
- **What does a check decide?** Spec Kit, command by command, with OpenSpec's verification as the contrast.
- **Who may act after merge?** Anthropic's playbook, with Microsoft's SRE Agent as the contrast.

Each case starts from what the documentation describes. Then it works through the assumptions the arrangement rests on and how it could fail, and labels that part as reasoning, not documentation.

**AI-native proposals differ in what they arrange, how far along the path they reach, and how much they implement; check what each one actually covers before comparing their claims.**

## Appendix: How the landscape was selected and classified

### Selection

Four criteria decide whether something counts as a proposal for this survey:

- **Publicly documented.** Public sources describe its mechanisms well enough to classify them. If access is limited, I look harder at what is public; that alone doesn't rule a proposal out.
- **Prescribes an arrangement.** A publication or built-in workflow says how lifecycle work is arranged across more than one duty: how activities and artifacts connect, how work is coordinated, and whose decisions let it proceed. A general coding agent alone doesn't qualify, however capable: doing the work isn't arranging it.
- **Relevant.** Its publisher presents it as a way to organize software delivery around AI agents, whatever it calls itself. Material only about building or evaluating AI products is out unless it also prescribes how agents take part in development work. Post 32 separated what runs the software from how AI helps build it.
- **Current.** Its publisher still offers it. A published method doesn't expire just because it has no new releases, but a withdrawn or paused one is left out.

Meeting the criteria isn't enough, because a landscape should cover what readers are likely to run into. So a proposal that meets them gets in when it has at least one visibility signal: a large technology company, a major consultancy, or an established framework body publishes it; at least 100 public repositories carried its files in a July 2026 census of spec-driven development [21]; or independent research on the field compares or discusses it by name. These signals are a rule of thumb. They show that readers may run into a proposal, not that it is popular, adopted, or good, and the 100-repository line is my working cutoff, not a finding of the census. A final check makes sure every kind of proposal and every pattern of reach is represented. That check can let in a distinctive proposal without a signal, as long as I record why; this time, none needed it. GSD Core gets in on the visibility of GSD, its archived predecessor.

I started from an AI-assisted web search for proposals from well-known companies and open-source communities, with documentation complete enough to read. Running the same search again returned a noticeably different set, so I don't treat either run as systematic. I added candidates from the census's list of tools and from published comparisons, checked each against the criteria, and kept a record of every decision. The result is a reviewed selection, not a census.

Two studies helped. *Spec-Driven Development as Prescribed and as Practiced* counted spec files in public GitHub repositories, with data collected through July 3, 2026, and interviewed 21 practitioners [21]. Its counts describe what code search could find at that date; they show use, not results. *From Prompt to Process* compares six frameworks that run on top of coding agents, scoring each on six process dimensions [22]. It is a preprint, and its author also created one of the six, which the paper discloses. Its scores are one person's judgments based on the documentation. It is useful for vocabulary and comparison, not as validation. A published comparison also names proposals included here: a Thoughtworks Technology Radar entry that compares OpenSpec with Spec Kit, Superpowers, BMAD, and Kiro [23].

Some candidates are left out, for these reasons. AWS's guidance on generative AI in the SDLC maps where AI can help; it doesn't arrange the work. IBM's and Salesforce's agent development lifecycles are about building AI agents as products. Smaller tools such as Zenflow, MoAI-ADK, SpecWeave, and Reversa meet the criteria but lack a visibility signal, as do academic proposals such as Hymel's V-Bounce model.

### Classification

Post 36 said this post would need its distinction between methods and tools, "because AI arrives both as methods and as tools, sometimes under the same name."

Post 36's three layers now read actors, methods, and tools. The first holds the duties and who performs them: roles group duties, and actors perform the activities a duty involves. Methods organize how that work connects, through handoffs, reviews, and decisions. Tools support or carry out parts of the arrangement. One thing can sit in more than one layer, depending on how closely you look at the process; this survey looks as closely as the proposals' own documentation does. A CI server is a tool a team sets up, and an actor when it runs the tests. An AI agent is an actor when a method assigns it an activity, even if a person reviews and owns the result. When the method gives the activity to a person who uses AI to do it, the agent is that person's tool. Some names span all three layers: Kiro brings agents, tools, and a workflow under one name. The four groups in the landscape follow from this distinction: they sort the eighteen by where the method lives and what carries it.

In more detail, the four groups are:

- **Methods only.** Anthropic's playbook [2], OpenAI's guide [9], Google's whitepaper on the new SDLC [10], and Scaled Agile's AI-Native SAFe [11] prescribe arrangements and leave implementation to the adopting organization, with general coding agents.
- **Methods with their own assets.** AWS AI-DLC [1], BMAD Method [13], Superpowers [14], Spec Kit [4], OpenSpec [15], Google's Conductor [8], Spec Kitty [16], and GSD Core [17] ship workflow files, skills, commands, or a command-line tool that carry the method into an agent's work. BMAD and Superpowers install as agent skills, Conductor is an extension for Gemini CLI, and Spec Kit is a method and its toolkit under one name.
- **Methods built into products.** Kiro, from AWS, is an agentic development product whose own spec workflow drafts requirements, a design, and tasks [6]. GitLab's Duo Agent Platform offers many task-specific flows; its Software Development Flow gathers context, creates a plan for review, works through it, and stages changes for the person to accept, modify, or reject [18].
- **Methods delivered through the publisher's platform.** Microsoft's Agentic DevOps [5], Atlassian's AI SDLC playbook [19], Thoughtworks' AI-first delivery [3], and Sonar's AC/DC [7] describe arrangements carried out through the publisher's own products and services.

Versions in the landscape are those reviewed in October 2026: the latest stable release for open-source projects and, for products, the documentation as I read it.

---

## References

Versions and pages were reviewed on October 23, 2026, and rechecked on October 24, unless noted.

[1] Raja SP, *AI-Driven Development Life Cycle: Reimagining Software Engineering*, AWS DevOps Blog, July 31, 2025. <https://aws.amazon.com/blogs/devops/ai-driven-development-life-cycle/>. Workflow files: awslabs/aidlc-workflows v2.11.1, October 20, 2026, <https://github.com/awslabs/aidlc-workflows/releases/tag/v2.11.1>; integrations in the README, <https://github.com/awslabs/aidlc-workflows/blob/v2.11.1/README.md>; the Operation stages are conditional, <https://github.com/awslabs/aidlc-workflows/blob/v2.11.1/docs/reference/04-stages/operation.md>. The method has three phases; the workflows have five.

[2] Louis Claxton, *The AI-native SDLC playbook*, Anthropic, August 21, 2026. <https://claude.com/resources/articles/the-ai-native-sdlc-playbook>. Recipes in the Claude Academy course, including hooks as approval gates, <https://academy.claude.com/courses/ai-native-sdlc-playbook/hooks-as-approval-gates>. Guidance and recipes informed by reported practice; the reviewed sources include no independent evaluation.

[3] Thoughtworks, *AI-first software delivery*, Looking Glass 2026, <https://www.thoughtworks.com/en-us/insights/looking-glass/looking-glass-2026/AI-and-software-delivery>, and AI/works technical guide, <https://www.thoughtworks.com/en-us/ai/works/technical-guide>. AI/works, launched January 20, 2026, supports Thoughtworks-led client delivery; outcomes are vendor-reported.

[4] GitHub, *Spec Kit*, v1.2.3, October 22, 2026. Quickstart: <https://github.com/github/spec-kit/blob/v1.2.3/docs/quickstart.md>. Open source (MIT).

[5] Microsoft, *Agentic DevOps: Evolving software development with GitHub Copilot and Microsoft Azure*, Azure Blog, May 19, 2025, <https://azure.microsoft.com/en-us/blog/agentic-devops-evolving-software-development-with-github-copilot-and-microsoft-azure/>. Operational component: *Azure SRE Agent overview*, Microsoft Learn, last updated August 27, 2026, <https://learn.microsoft.com/azure/sre-agent/overview>; run modes: <https://learn.microsoft.com/azure/sre-agent/run-modes>. The arrangement is classified from the announcement; the overview doesn't use the term.

[6] Kiro (AWS): homepage, <https://kiro.dev/>; documentation, including Specs <https://kiro.dev/docs/specs/>, Quick Spec <https://kiro.dev/docs/specs/quick-spec/>, Correctness <https://kiro.dev/docs/specs/correctness/>, Workflows <https://kiro.dev/docs/workflows/>, Hooks <https://kiro.dev/docs/hooks/>, and Permissions <https://kiro.dev/docs/permissions/>. IDE release 1.4.26, October 22, 2026. Capabilities differ across the IDE, command line, and web; the current documentation differs from the July 2025 preview.

[7] Sonar, *About the Agent Centric Development Cycle*, documentation, <https://docs.sonarsource.com/agent-centric-development-cycle/>; press release, *Sonar Introduces the 'Agent Centric Development Cycle' for the Next Era of Software Development*, March 3, 2026, <https://www.sonarsource.com/company/press-releases/sonar-introduces-the-agent-centric-development-cycle/>. Launched with four stages; the current documentation describes three loops, carried out through Sonar's products.

[8] Google, *Conductor*, v0.4.1, March 11, 2026, a Gemini CLI extension (Apache 2.0): <https://github.com/gemini-cli-extensions/conductor/blob/conductor-v0.4.1/README.md>. Announcement: Keith Ballinger, Jay Kornder, and Sherzat Aitbayev, *Conductor: Introducing context-driven development for Gemini CLI*, Google Developers Blog, December 17, 2025, <https://developers.googleblog.com/conductor-introducing-context-driven-development-for-gemini-cli/>. The unreleased default branch describes a plugin for several coding agents.

[9] OpenAI, *Building an AI-Native Engineering Team*, undated web guide. <https://learn.chatgpt.com/docs/guides/build-ai-native-engineering-team>.

[10] Addy Osmani, Shubham Saboo, and Sokratis Kartakis, *The New SDLC With Vibe Coding*, Google whitepaper for the Kaggle course of June 15–19, 2026. <https://www.kaggle.com/whitepaper-the-new-SDLC-with-vibe-coding>. Coauthor's account, *The New Software Lifecycle*, June 16, 2026: <https://addyosmani.com/blog/new-sdlc-vibe-coding/>. The paper's full body wasn't accessible for this review; the entry is classified from the coauthor's account. Deployment, with observability, appears only in its passage on building an agent as the product; I read its testing loop, which ends "watch production for new ones," as applying to such agents too.

[11] Scaled Agile, *AI-Native SAFe*, unveiled June 16, 2026; AI Innovation Pipeline, updated August 11, 2026. <https://framework.scaledagile.com/ain-safe-ai-innovation-pipeline>. Detailed guidance is partly behind a login.

[12] Madhu Chebrolu, Aleister Cachon Guillen (Dynata), Naga Gaddamu, and Satish Bhonsle (AWS), *How Dynata scaled AI across the development lifecycle with AI-DLC and Kiro IDE*, AWS Architecture Blog, October 15, 2026. <https://aws.amazon.com/blogs/architecture/how-dynata-scaled-ai-across-the-development-lifecycle-with-ai-dlc-and-kiro-ide/>. The figures are Dynata's estimates; there is no controlled comparison.

[13] BMAD Method, v6.12.4, October 14, 2026. <https://github.com/bmad-code-org/BMAD-METHOD/tree/v6.12.4>. Build workflow: <https://github.com/bmad-code-org/BMAD-METHOD/blob/v6.12.4/docs/build/build-a-change.md>; organizational sign-off: <https://github.com/bmad-code-org/BMAD-METHOD/blob/v6.12.4/docs/plan/plan-inside-an-organization.md>; retrospective: <https://github.com/bmad-code-org/BMAD-METHOD/blob/v6.12.4/docs/build/finish-an-epic.md>. Open source (MIT). A V7 preview is on the default branch.

[14] Superpowers, v7.0.1, October 21, 2026. <https://github.com/obra/superpowers/blob/v7.0.1/README.md>. Open source (MIT); the default workflow ends at branch completion.

[15] OpenSpec (Fission-AI), v1.15.0, October 24, 2026. <https://github.com/Fission-AI/OpenSpec/tree/v1.15.0>. Skill contracts, including verification: <https://openspec.dev/docs/skills>. Open source (MIT).

[16] Spec Kitty, v3.2.7, September 9, 2026. <https://github.com/spec-kitty/spec-kitty/blob/v3.2.7/README.md>. Open source (MIT). A 4.0 release candidate is on the default branch.

[17] GSD Core, v1.18.0, October 19, 2026. <https://github.com/open-gsd/gsd-core>. Verify and ship: <https://github.com/open-gsd/gsd-core/blob/v1.18.0/docs/how-to/verify-and-ship.md>. Open source (MIT). It continues GSD, whose repository was archived on June 26, 2026: <https://github.com/gsd-build/get-shit-done>.

[18] GitLab, *Software Development Flow*, <https://docs.gitlab.com/editor_extensions/software_development/>, and *GitLab Duo Agent Platform*, <https://docs.gitlab.com/user/duo_agent_platform/>, documentation read October 23, 2026. The flow, formerly GitLab Duo Workflow, became generally available in GitLab 18.8 and runs in VS Code, Visual Studio, and JetBrains IDEs. The entry is classified from this flow; the platform's other flows each address one task.

[19] Atlassian, *The AI SDLC transformation playbook*, October 1, 2026. <https://www.atlassian.com/blog/ai-at-work/ai-sdlc-transformation-playbook>.

[20] Atlassian, *The AI-native SDLC is paying off: 19% more PRs and 2–3 hours saved per developer per week*, May 31, 2026. <https://www.atlassian.com/blog/ai-at-work/ai-native-sdlc-paying-off-per-developer-per-week>. Propensity-matched comparison of 3,400 repositories from 2,500 customers, plus a survey of more than 6,200 Atlassian developers; measured by the publisher, and earlier than the October playbook.

[21] *Spec-Driven Development as Prescribed and as Practiced*, arXiv 2608.25202, version 4, October 15, 2026. <https://arxiv.org/html/2608.25202v4>. A census of spec files in public GitHub repositories (collection ended July 3, 2026), with a Reddit analysis and 21 interviews. It covers files that code search can find and shows visibility and use, not market share or outcomes.

[22] Sanderson Oliveira de Macedo, *From Prompt to Process: a Process Taxonomy and Comparative Assessment of Frameworks Supporting AI Software Development Agents*, arXiv 2606.04967, version 1, June 3, 2026. <https://arxiv.org/html/2606.04967v1>. A preprint. Its author also created Reversa, one of the six frameworks compared, and discloses this; its scores are one person's judgments from documentation, not an independent measurement.

[23] Thoughtworks, *Technology Radar*: OpenSpec, Assess, April 15, 2026. <https://www.thoughtworks.com/en-us/radar/tools/openspec>.

## Earlier in this series

All posts are archived in the GitHub repository: <https://github.com/young-rex/notes-on-agentic-ai>

- Post 36: *The SDLC We Already Practice, Before AI* <https://lnkd.in/p/gtYC_M4g>
- Post 35: *Developing C2 and C3 Software with AI: Today* <https://lnkd.in/p/gjq3THpk>
- Post 33: *Developing C1 Software with AI: Today and the Path Ahead* <https://lnkd.in/p/gE9xvGSd>
- Post 32: *AI Coding Has Two Sides. Ours Starts with What Runs the Software.* <https://lnkd.in/p/gacN7SgW>
- Post 30: *AI Coding Has Two Sides. First, Theirs.* <https://lnkd.in/p/gyg_buW9>
- Post 16: *Calibrate: Choose a Model for Enterprise Refund Decisions* <https://lnkd.in/p/gk_7yGX5>
