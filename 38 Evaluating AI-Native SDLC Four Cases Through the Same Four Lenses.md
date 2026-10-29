# 38. Evaluating AI-Native SDLC: Four Cases Through the Same Four Lenses

*Originally published on LinkedIn: <https://lnkd.in/p/gRgZVnD4>*

## 1. The same refund change, with agents on the path

A retailer changes its refund policy. Today, customers get a full refund within 30 days of delivery. From March 1, they still get a full refund within 30 days, but from day 31 to day 60 they get store credit instead. Defective items can be refunded for 90 days, final-sale items can't be returned, and orders placed before March 1 keep the old rule. A refund over $500, or a customer's third refund in 12 months, goes to a person for review.

Post 36 followed this change through the SDLC as people already practice it. This post puts agents on the same path and asks what changes when a participant can perform substantial duties but sees and acts only through the context, tools, and permissions it is given. Of the eighteen AI-native proposals Post 37 mapped, this post follows four. Here, as in Post 36, the refund rule lives in code; when a model decides the cases instead, as in Post 16, validating a policy change takes evaluation, which lies outside these four cases.

The failure modes are my deductions, each stated with the conditions that would trigger it; none is an observed result, and the reviewed sources neither confirm nor rule them out. One disclosure: I have a proposed method of my own, which the next post examines through the same lenses and compares with some of these cases.

**The question isn't whether an agent can take on a duty, but how the arrangement around it changes when it does.**

## 2. Four lenses for evaluating each case

Context, coordination, checks, and authority were needed long before agents; Post 36 showed how people and existing automation provide them. Post 37 counted checks and authority as one lens; here they are separate, because the cases show that what produces evidence and who may decide can come apart. All four apply to every case:

- **Context:** What information can the agent reach, and what relevant evidence stays outside its view?
- **Coordination:** What chooses the next step, and how is the agent's work coordinated with other duties?
- **Checks:** What does each check establish, who or what judges it, and does it report, ask, stop, or create more work?
- **Authority:** Who may decide, approve, or act, what does each approval cover, and what enforces those limits?

Each case also takes up the question Post 37 set for it, and each has a contrast:

- **AWS's AI-DLC:** Who decides when work is split and gated? Kiro's spec workflow is the contrast.
- **BMAD:** What does a role mean when an agent plays it? My own proposal, Lemina SDLC, is the contrast.
- **GitHub's Spec Kit:** What does a check decide? OpenSpec's verification is the contrast.
- **Anthropic's AI-native SDLC playbook:** Who may act after merge? Microsoft's Azure SRE Agent is the contrast.

For each case, I examine what the documentation describes, what the arrangement is meant to achieve, what it depends on, what differs from Post 36's baseline, and how it could fail under stated conditions. The same refund change keeps the business problem constant while the arrangements vary.

**The needs aren't new; how they're met changes.**

## 3. Case 1: AI-DLC splits the work and waits for approval

### What it proposes

AWS introduced its AI-Driven Development Life Cycle (AI-DLC) in July 2025 with one repeated pattern: "AI creates a plan, asks clarifying questions to seek context, and implements solutions only after receiving human validation." In its first phase, "AI transforms business intent into detailed requirements, stories and units through 'Mob Elaboration' – where the entire team actively validates AI's questions and proposals." Sprints give way to "bolts," work cycles "measured in hours or days rather than weeks" [1].

The intended benefit is clear. In NaPiRE, a survey of requirements engineering in 228 companies, incomplete or hidden requirements were the problem respondents named most often [2]. An agent that writes down "Does day 31 count from purchase or from delivery?" before anyone writes code, and a gate that records the answer, are aimed squarely at it.

The open-source workflow files that carry the method into an agent make the pattern concrete [1]. The agent proposes a scope, a named plan of stages, from the request, and the person confirms it; later, the agent splits the work into units, and the person approves the split at that stage's gate. On an existing codebase, the full feature scope runs thirty stages after setup, with thirty approval gates. When a stage needs input, the agent writes its questions to a file, and each names who would usually answer it, such as **[Product owner]** or **[Developer]**; "Anyone can still answer any question." A human-presence guard refuses "an approval or an answer with no real human turn behind it," and hooks stop the agent's own tools from writing the records of those approvals, though the project calls its audit write guard "a guardrail, not a security boundary."

Settings decide how much of this holds. In the Construction phase, "Continue automatically" waives the ordinary completion checkpoints. Each code plan still waits for approval while plan approval is on, as it is on every shipped scope except **express** and **poc**, and every failure still goes to the person. A Guard Policy decides what happens when "something you already approved turns out to have changed underneath," such as a reviewed document edited after its review. Set to **strict**, it "reopens the approval." Under **relaxed** or **off**, the edit is recorded in the audit trail, "you hear one line about it, and the run continues"; for an edited plan, the agent also offers to go back to the approved one. The approval isn't stretched to cover the edit: "your original answer and approval evidence remain a record of what you actually approved." Every shipped scope except **enterprise** starts with **off**. The default scope, **classic**, skips all seven Operation stages, as do **mvp** and **poc** [1].

### Where it could fail

As I read it, carrying this refund change to business acceptance depends on several things holding: whoever answers a labeled question holds the authority the label names, each approval reflects a reading, a material edit to approved work gets reviewed again, and each affected duty is covered by the chosen scope or assigned elsewhere.

What differs from Post 36's baseline? There, the question surfaced in conversation, and the organization decided who answered it. Here, it surfaces systematically, a label routes it, and the audit trail records a person's reply; its documented entries don't say who replied, or with what authority. My deductions:

- **The wrong person answers.** If the developer running the session answers the **[Product owner]** question to keep moving, the gate records a policy decision made without policy authority, and the record as documented wouldn't show it.
- **Approval by habit.** Thirty gates are thirty chances to read carefully, or to click through. If the approver is also the person driving the session, approval can become a reflex the record can't tell apart from judgment.
- **A notice that goes unread.** If the business settles the counting rule after the requirements were approved, and someone edits them, the run continues under the default policy with a one-line notice. The record stays honest about what was approved. The failure comes if the notice is missed or dismissed without renewed approval, and the work goes on as if the edited requirement had been approved.
- **Operation drops out with the scope.** If the refund change runs on the default scope, the dashboards and support tickets Post 36 relied on are outside the workflow. The confirmation states how many stages will run, but a count doesn't say what's missing.

### The contrast: Kiro

Kiro's own spec workflow draws the line differently [3]. In AI-DLC, gates are the default, and lowering one is a recorded setting; in Kiro, the route decides whether intermediate gates exist at all, and Quick Spec generates the requirements, design, and tasks "with no approval gates between phases." The refund change looks small, which makes Quick Spec tempting, yet the change's open questions are what an intermediate review is for. Kiro writes requirements in EARS, a structured style published in 2009 [4], and its optional property-based tests are, in its own words, "evidence of correctness, not a proof" [3]. No test can check a day-31 return until someone decides where day 31 starts. A test can expose a missing decision; it can't make it.

**The route and settings determine when approval is required and whether an edit triggers another review; a gate shows that a person answered, and the organization decides who may answer and what an approval covers.**

## 4. Case 2: BMAD gives agents roles, not business authority

### What it proposes

The open-source BMAD Method installs five named agents: an Analyst, a Product Manager, an Architect, a Developer, and a UX Designer, each "an identity plus a customizable layer" with its own menu of workflows [5]. Its README promises "Specialized perspectives," bringing "product, architecture, UX, development, and testing expertise when it helps," and collaboration "without handing over judgment." Testing comes through the Developer, whose menu includes test generation.

People keep the documents. BMAD's guide for organizations assigns each document to a human role: the product manager owns the product requirements document (PRD), the designer the UX documents, the tech lead or architect the architecture, and, for each epic, one engineer owns its specification, its stories, and "its verdict." Each document "has exactly one owner, because each has exactly one skill that writes it." The guide names "five moments where a human decision is expected, and each one blocks something specific," and tells organizations to "Put your existing approvals at those points." One is a readiness gate: "Could a developer implement these stories without inventing decisions." The last is a retrospective verdict: "Did the epic meet its own acceptance criteria." BMAD calls the five "sign-off moments," yet two of them, including the retrospective, "can run unattended" [5].

The retrospective shows how far an agent's verdict reaches. It returns "accepted," "accepted-with-open-items," or "rejected"; unfinished stories make it reject, and "a human can still override." "A failing epic never closes as quietly accepted." Run unattended, it gives a "verdict on the evidence alone," and "The verdict is the gate" for the next epic [5].

BMAD is candid about one limit. Its multi-agent discussions default to one model voicing every persona, and the documentation warns: "One model voicing five personas tends to make them agree." Separate agents, it adds, keep their reasoning independent, at a higher cost [5].

The intended benefit is a team's range of perspectives for one builder or a small group, with decisions made explicit and documents that agree because each has one writer and one owner.

### Where it could fail

As I read it, carrying this refund change to business acceptance depends on several things holding: the personas can reach the records people in those roles would see, their agreement reflects review rather than one model's assumption, document owners route business decisions to the people authorized to make them, and the acceptance criteria capture what the business needs.

What differs from Post 36's baseline? There, if the analyst, the product owner, the architect, and the tester were different people, each saw a different part of the path. That separation cost information, and it also caught gaps: a tester's question sent the team back to the rule. If one person held several of those roles, as with one builder or a small group, some of those views may never have been voiced. Either way, BMAD makes the perspectives available on demand, without the people. A persona brings a role's point of view, not what the person in that role also had: the support tickets they saw, the finance report they knew about, and the authority the organization gave them. The Analyst knows only what it is given. My deductions:

- **Agreement that isn't review.** The policy doesn't say whether a defective final-sale item can be refunded. If the Product Manager and the Architect, voiced by one model in one session, both assume the defect rule wins, their agreement is one assumption stated twice. BMAD's own warning describes the condition.
- **Independence without evidence.** Separate agents keep their reasoning independent, though in every mode the result "reads as one conversation." Any of them may ask about the finance reporting or the support history. But independent reasoning doesn't supply missing business evidence: if none of them has those records, none can say what they show.
- **A decision made at the keyboard.** If a persona proposes an answer to the final-sale question and the engineer accepts it to keep the build moving, whoever runs the session has settled the rule. BMAD's rule that a missing answer "goes into the PRD first" puts it in front of the PRD's owner, but the product manager who owns the document may not own the refund policy.
- **An acceptance the criteria made easy.** BMAD guards one direction: a failing epic can't close quietly. If the acceptance criteria left out grandfathered orders, an unattended retrospective could accept the epic on the evidence alone, correctly by the criteria and wrongly for the business.

The refund policy's owner isn't a persona, and isn't in BMAD's table of document owners either. Placing that person at the right decision point is the organization's assignment.

### The contrast: Lemina SDLC

My proposed method, Lemina SDLC, keeps the delivery roles with people: each owns an artifact and chooses whether to use AI in their work. BMAD gives agents professional identities and assigns documents to human owners; mine organizes the principal handoffs around those human-held roles. Human ownership alone establishes neither careful review nor authority over the refund policy. In the next post, I examine this proposal, including its bet that small reviews in each owner's role can stay genuine as AI speeds up drafting.

**A persona brings a role's point of view and only the evidence it is given; whether agents play roles or people hold them, authority over business policy stays with the people the organization assigns.**

## 5. Case 3: Spec Kit checks the work against its own artifacts

### What it proposes

GitHub's Spec Kit turns a feature into a specification, a plan, and tasks, and its checks carry different kinds of authority [6]. Analyze is read-only: it reports "conflicts, gaps, and ambiguities across **spec.md**, **plan.md**, and **tasks.md**." Checklists are "unit tests for your requirements," marked by a reviewer, and "Checked custom items do not mean implementation work is complete." Implement reads the checklists "as a gate": its command template tells the agent to "**STOP** and ask" whether to proceed when items are unchecked, and to halt if the answer is no.

Converge goes furthest. It checks "the codebase against the spec, plan, and tasks," reading them as "the sole source of intent," and limits its assessment to the files they name, plus a search for the concepts in each requirement. For each gap, it "appends new tasks to **tasks.md**"; it never edits the specification or the code. With nothing left, it reports "Converged — the implementation satisfies the spec, plan, and tasks," and the quickstart's next step is to "proceed to review or open a PR."

Each check has its own intended benefit: inconsistencies caught before code, requirement quality judged by a person, implementation paused for an explicit decision when checklist items remain unchecked, and remaining work found and queued without anyone hunting for it.

The illustrations are mine. Analyze could report that the specification counts day 31 from delivery while a task counts from purchase. Implement could stop because the checklist item for defective final-sale items is unchecked. Converge could find grandfathering specified but absent from the code, and append a task to build it.

### Where it could fail

As I read it, carrying this refund change to business acceptance depends on the artifacts: the specification, plan, and tasks have to say what the business meant and name everything the change touches.

What differs from Post 36's baseline? There, tests, continuous integration, and review produced evidence, and people, or the processes they set up, made the decisions. Here, the agent can run a check at every step, several checks are judgments it makes by following a command template, and one writes new work. A test suite reports that the code fails; Converge decides what is left to do. My deductions:

- **"Converged" read as correct.** Converged means the code satisfies the artifacts, as the agent judged them. If the specification says "from purchase" and the business meant "from delivery," the loop converges on the wrong rule. And if finance reporting needs to change but appears nowhere in the specification, plan, or tasks, Converge has no requirement to hold it to, whatever its searches reach.
- **Gaps closed in one direction.** Converge can flag code that contradicts the specification, but it never edits the specification, so the work it appends moves the code toward the artifacts. If the artifacts were wrong, each round brings code and artifacts closer to each other and no closer to the business.

### The contrast: OpenSpec

OpenSpec's optional verification compares the artifacts with the implementation and reports on completeness, correctness, and coherence [7], but it "changes nothing and does not archive," and it "does **not** block archiving — it surfaces the gaps and leaves the call to you." Where Spec Kit's Converge turns a gap into a task, OpenSpec's verification turns it into a report without blocking what comes next. My reading: both leave the next decision to people, and because both take the artifacts as the reference, both can follow a mistaken one.

**Checks can report, ask, or write new work; agreement with the artifacts doesn't establish that they say what the business meant.**

## 6. Case 4: Anthropic grades the agent's authority after merge

### What it proposes

Anthropic's AI-native SDLC playbook carries the work past merge [8]. Its plays are "grouped into six non-linear stages (Plan, Design, Build, Test, Deploy, Maintain)," and the chain of artifacts it keeps in version control starts with an **intent.md**. Its governing principle: "the agent may act up to the production gate and cannot pass it." The production deploy hook "blocks the release until a named release manager authorizes it."

In production, a deterministic script watches control bands, thresholds measured in standard deviations (σ) from normal: "At 1σ the script only logs, at 2σ it invokes Claude read-only to diagnose, and at 3σ Claude may act, though only by opening a PR into the review gate or triggering a pre-approved runbook." The runbooks it may trigger are scripted operating procedures, approved in advance. A diagnosis comes back as a new **intent.md**, the start of the next piece of work. The playbook's examples are engineering metrics, such as the CI test failure rate and the post-deploy server error rate [8].

The intended benefit is a clear division of labor: the agent prepares each release and investigates signals, a named person authorizes the release, and the agent's authority grows with the strength of the signal. To watch the refund rule rather than the servers, the organization would add its own bands for the refund rate, store-credit issuance, and support tickets.

If day-31 tickets climb past their band, the loop opens new work, such as a pull request through the review gate to clarify the checkout message. A rise in complaints can start an investigation or a proposal to revisit the rule; it doesn't authorize changing the policy.

### Where it could fail

As I read it, carrying this refund change to business acceptance depends on several things holding: the agent can't write to the records its gate reads, the bands measure what matters to the business and are reset when normal changes, and a release approval isn't read as approving the policy.

What differs from Post 36's baseline? There, alerts were already automated; they went to whoever ran the service, and that person judged what to do. Here, the band that raises a signal also sets what the agent may do about it. The judgment about what counts as abnormal, and how much action it warrants, moves into the bands' calibration. My deductions:

- **A gate the agent can write to.** The playbook's approval-gate recipe states the condition itself: "The gate depends on the agent having no way to write to the records it reads." If the release manager's approval is recorded where the agent's credentials can write, the gate is decorative.
- **Bands that read the policy as an anomaly.** The new rule creates a flow that didn't exist before: store credit for returns from day 31 to day 60. If the store-credit band was calibrated on history from before March 1, the rule's intended effect could breach it, and the agent would be called to diagnose the policy working as designed. Resetting the bands is a business judgment about what normal now means.
- **Business errors that set off no band.** If only engineering metrics are banded, store credit issued wrongly on a grandfathered order raises no error rate. The rule can fail while every band stays green.
- **A release read as a policy decision.** A recorded release approval authorizes this release, not the refund policy. And an automated rollback undoes code, not its effects: as Post 36 noted, turning the feature flag off doesn't take back store credit already issued.

### The contrast: Azure SRE Agent

Microsoft's Azure SRE Agent sets authority another way [9]: "Whether the agent applies a mitigation on its own depends on the run mode you set for the response plan or scheduled task." In Review mode, an administrator approves its Azure infrastructure write actions first; other actions "proceed based on the agent's reasoning and your response plan instructions." In Autonomous mode, it doesn't wait. Anthropic grades authority by the size of the signal; the SRE Agent sets it per plan, in advance. My deduction: switching a plan to Autonomous keeps its tools and removes the wait, so the switch is an authority decision worth revisiting whenever the plan's reach grows.

**After merge, an agent's authority can be graded by signal or set by mode; either way, it holds only while the agent can't alter the controls behind it, and it doesn't extend to the policy.**

## 7. What changes, and what doesn't

Lens by lens, the four cases show what changes when an agent takes the steps.

- **Context is packaged for the agent.** A colleague may bring context from other work; an agent works from what is written into the artifacts it reads or reachable through the tools it's given. AI-DLC writes questions to a file, BMAD routes answers into the PRD, and Converge takes the artifacts as its source of intent. Records the agent can neither read nor reach, such as the finance reporting or the support history, stay outside its evidence.
- **Workflows can also coordinate the agent's own steps.** AI-DLC's engine chooses the next stage, and Anthropic's script decides when the agent is called. In Spec Kit and BMAD, people still choose which command or skill runs next.
- **Checks multiply, and many are model-judged.** Some report, some ask, some never block, and one creates work. Each needs a person who knows which kind it is.
- **Authority is configured around the agent.** Gates, guards, hooks, run modes, and bands limit the agent's actions and extend control from the work's results to the agent's own steps. They hold only while the agent can't alter the authorization records and control settings they depend on. Some can be lowered by a setting or a choice of route, such as "Continue automatically," Quick Spec, and Autonomous mode; some start lowered, such as a Guard Policy that starts at **off**. Others sit outside the session's reach: AI-DLC's human-presence guard has no per-work switch, and Anthropic puts non-negotiable hooks in managed settings "where individual engineers cannot switch them off" [8]. A setting that lowers a control is an authority decision that looks like a preference.

The four arrangements also change who reviews what: thirty stage approvals in AI-DLC, five sign-off moments in BMAD, reviewer-marked checklists in Spec Kit, and a named release manager in Anthropic's playbook. The counts don't measure effort: approving one stage of a run, signing off on a decision that blocks the next step, and authorizing a release are different judgments, and they can fall to different people.

What doesn't change are the decisions Post 36 found along the path: what behavior the business intends, whether the result is accepted, whether the release ships, and whether the policy changes. An arrangement can record an approval or block an action, but role labels and approval records don't, by themselves, establish who in the business holds the authority to give that approval.

This series defines an AI-native SDLC as one whose methods, languages, and toolchains are designed for AI rather than for people. By that definition, my reading is mixed. The engines, guards, question labels, personas, command templates, hooks, and bands are methods and toolchains designed around an AI actor, and here they also depend on one: without an agent, most would have nothing to coordinate or control. A language designed for AI would give agents a structure to edit and tools a way to check every edit, with a readable view for people. The artifacts these arrangements pass along, such as requirements, user stories, PRDs, and specifications, are still prose that people write and approve; even EARS, a structured style, was published in 2009 for human writers [4]. These arrangements redesign coordination and control around the agent, while the languages of the work stay largely those designed for people.

When someone proposes an AI-native change to your development process, ask what moves to the agent, what context it can reach, what chooses the next step, what runs unattended, what can stop the work, and which settings lower that stop and who controls them. Also ask how much review it requires and from whom, what each approval covers, what a later edit requires, and who accepts the result. Then ask Post 36's three questions of each answer: is it described, is it implemented, and has it been shown to work, where, and for what? The next post applies the same lenses to my own method.

**Agents take the steps, with context packaged for them, coordination encoded in workflows, many checks judged by models, and permissions configured around them; people still assign authority and accept business results.**

---

## References

Unless otherwise noted, sources were consulted on October 23, 2026, and rechecked on October 24.

[1] Raja SP, *AI-Driven Development Life Cycle: Reimagining Software Engineering*, AWS DevOps Blog, July 31, 2025. <https://aws.amazon.com/blogs/devops/ai-driven-development-life-cycle/>. Workflow files, awslabs/aidlc-workflows v2.11.1, October 20, 2026. <https://github.com/awslabs/aidlc-workflows/tree/v2.11.1/docs/guide>. Pages consulted: the glossary; scopes and their approval gates; question labels; "Continue automatically"; Guard Policy; the audit trail and its entries. The guards are enforced by hooks, and the project's facilitator guide warns that "A machine whose hooks never run reports nothing." The 2025 method and the current workflow files differ.

[2] Daniel Méndez Fernández et al., *Naming the Pain in Requirements Engineering: Contemporary Problems, Causes, and Effects in Practice*, Empirical Software Engineering, 2017. <https://arxiv.org/abs/1611.10288>. A survey of 228 companies in 10 countries, run in 2014–15; respondents chose their five most critical problems from a list of 21 problems. The authors can't estimate how representative the sample is.

[3] Kiro (AWS) documentation, IDE release 1.4.26, October 22, 2026. <https://kiro.dev/docs/specs/>. Pages: Feature Specs, Quick Spec, and Correctness. Property-based tests are listed for the IDE only; the current documentation differs from the July 2025 preview.

[4] Alistair Mavin, Philip Wilkinson, Adrian Harwood, and Mark Novak, *Easy Approach to Requirements Syntax (EARS)*, IEEE International Requirements Engineering Conference (RE), 2009. <https://research.manchester.ac.uk/en/publications/easy-approach-to-requirements-syntax-ears/>.

[5] BMAD Method, v6.12.4, October 14, 2026. <https://github.com/bmad-code-org/BMAD-METHOD/tree/v6.12.4>. Pages: the README, agents, document owners and sign-off, the retrospective, and multi-agent discussions. Open source (MIT). A v7 preview is on the default branch.

[6] GitHub, *Spec Kit*, v1.2.3, October 22, 2026. <https://github.com/github/spec-kit/tree/v1.2.3>. Pages: the quickstart and the command templates for Implement and Converge. Open source (MIT). The checks run as the agent follows the command templates.

[7] OpenSpec (Fission-AI), v1.15.0, October 24, 2026. <https://github.com/Fission-AI/OpenSpec/tree/v1.15.0>. Page: *Reviewing a Change*, on verification and archiving. Skill contracts, including verification: <https://openspec.dev/docs/skills>. Open source (MIT). Verification is in the expanded profile, not the core one.

[8] Louis Claxton, *The AI-native SDLC playbook*, Anthropic, August 21, 2026. <https://claude.com/resources/articles/the-ai-native-sdlc-playbook>. Recipes in the Claude Academy course, including hooks as approval gates: <https://academy.claude.com/courses/ai-native-sdlc-playbook/hooks-as-approval-gates>. The guidance and recipes draw on reported practice; the reviewed sources include no independent evaluation.

[9] Microsoft, *Azure SRE Agent documentation*, Microsoft Learn. <https://learn.microsoft.com/azure/sre-agent/>. Pages: overview (last updated August 27, 2026) and run modes. The operational component of Microsoft's Agentic DevOps, as classified in Post 37.

## Earlier in this series

All posts are archived in the GitHub repository: <https://github.com/young-rex/notes-on-agentic-ai>

- Post 37: *AI-Native SDLC: The Landscape* <https://lnkd.in/p/gkDevySp>
- Post 36: *The SDLC We Already Practice, Before AI* <https://lnkd.in/p/gtYC_M4g>
- Post 16: *Calibrate: Choose a Model for Enterprise Refund Decisions* <https://lnkd.in/p/gk_7yGX5>
