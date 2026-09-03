# The Enterprise Chose Speed. Own the Burden.

*Originally published on LinkedIn: <https://lnkd.in/p/gsv6FUFr>*

Post 11 showed that code can check what crosses the edge between the two stacks — the shape, the schema, the format. It cannot check the judgment inside. That judgment used to be a human's. The enterprise offloaded it to AI.

When it is wrong, who answers for it?

## At human pace, human review still works

This is the part nobody says out loud.

When a human and AI work together at a pace that preserves the sequence — write, review, revise — the human has a meaningful opportunity to catch errors. Not all of them, but the arrangement can function. The human is in the loop in the way the phrase actually means: present, reviewing, able to intervene before consequences.

Liang models one mechanism as the novelty bottleneck [1]: the fraction of a task requiring human judgment is an irreducible serial component, analogous to Amdahl's Law. Better agents improve the coefficient on human effort, not the exponent. It is a stylized framework, not proof that human effort must always scale this way.

At human pace, that serial component fits. The human has time.

## The enterprise chose speed

Enterprises did not deploy AI to work at human pace. They deployed it for throughput.

The results are documented. Faros's 2025 telemetry study of more than 10,000 developers across 1,255 teams found that developers on high-AI-adoption teams completed 21% more tasks and merged 98% more pull requests — while PR review time was 91% longer [2]. These are observational relationships across adoption levels, not causal estimates.

LinearB's 2026 analysis of 8.1 million pull requests across 4,800 engineering teams in 42 countries found that AI-generated PRs waited 4.6 times longer for reviewer pickup; fully agentic PRs waited 5.3 times longer than unassisted ones [3]. Within 30 days, 32.7% of AI-generated PRs were accepted, against 84.4% of unassisted PRs.

He et al. found a related pattern in matched open-source GitHub projects. Their preferred difference-in-differences estimator found a significant but transient velocity increase — alongside a persistent 30.3% increase in static analysis warnings and 41.6% increase in code complexity [4]. The quality estimates varied across alternative estimators. Their practical conclusion: quality assurance needs to scale with AI-era velocity.

The upstream got faster. The downstream — where a human has to read, understand, and decide — did not.

## At AI speed, human review cannot keep up

This is not a skill problem. It is arithmetic.

When output volume doubles and review capacity stays flat, the human is no longer in the loop. They are still in the org chart. They are still nominally responsible. But the loop now runs faster than they can see.

"Keep a human in the loop" assumes the loop waits for the human. At AI speed, it does not.

The enterprise faces a choice it has mostly not acknowledged. Slow down to human pace and keep the arrangement that works. Or run at AI speed and accept that human review of every output is no longer possible.

When an enterprise maximizes throughput without expanding review capacity, it has chosen the second in practice, whether or not it says so.

## Control did not disappear. It moved.

Once per-output human review is no longer feasible, real control lives in different places:

- **deployment authority** — whether this system runs in this context at all
- **scope and guardrails** — what the system is and is not permitted to do
- **monitoring** — what is measured, how often, and against what baseline
- **intervention** — the ability to stop, pause, or route around
- **rollback and remediation** — undoing effects and repairing damage after the fact

None of these requires reading every output. All of them require authority someone has to hold.

An organization that has automated per-output review out of existence has not lost control. It has moved control to these five places — and has usually not said out loud who holds them.

## Blame breaks the feedback loop

Responsibility is a feedback signal. Its function in an organization is to route consequences back to a point where behavior can change.

Assign it to someone who holds none of these five levers, and the signal arrives at a node with no actuator. The loop does not close. The organization gets punishment without learning — the failure is attributed, the attribution is recorded, and nothing becomes more likely to catch the next one.

This is a wrong-tool error, and this series already wrote the rule it breaks. Post 4 argued that a role is a bundle of duties, and that the duties suited to AI are those with an expressible target and checkable output. Post 11 said how far "checkable" extends: to the edge between the two stacks, and no further.

Allocating a duty whose unchecked judgment exceeds what the human can review is not a new kind of mistake. It violates the allocation rule this series set several posts ago. Having someone to blame afterward does not fix the mismatch.

## Everyone owns a piece. Nobody owns the whole.

McKinsey's November 2025 survey found that 51% of respondents from organizations regularly using AI reported at least one negative consequence [5]. A separate McKinsey survey published in March 2025 found that 28% of respondents said the CEO was responsible for overseeing AI governance; respondents reported an average of two leaders jointly owning governance [6]. McKinsey also found CEO oversight associated with stronger reported financial impact. The evidence points to shared and uneven ownership, not an absence of owners.

Schellman's 2026 governance survey found 42% naming the CIO or head of IT as primarily responsible for AI purchasing decisions, and 37% naming that role as ultimately accountable when AI risks emerge [7]. These aggregate figures do not show that the same person holds both functions in the same organization. They do raise a governance question: is risk challenge sufficiently independent of purchasing?

Chung and Spisak put it structurally: removing layers of human judgment yields short-term efficiency while eliminating the organizational buffers that once dispersed responsibility [8].

Nobody arranged this. It is what happens when nobody asks.

## The enterprise chose speed. The employee did not.

The enterprise chose to deploy AI at AI speed. That choice made per-output human review impossible. The judgment that code cannot check and humans can no longer review does not vanish. It lands somewhere.

By default, it lands on the nearest employee. Elish documented the pattern in automated systems and named it: a *moral crumple zone* — when a human–automation system fails, responsibility settles on the nearest human, often the one with the least real control [9]. Applying the concept to enterprise AI extends that pattern into a new setting.

The vendor side can reinforce the drift. A product warning that tells users to verify AI output pushes the verification burden downstream: from vendor to deploying organization, from organization to whoever is nearest the output. If the enterprise does not catch it there, it lands on the employee.

But the employee did not choose AI speed. The enterprise did. The employee did not design the deployment scope, the guardrails, the monitoring, or the escalation path. The enterprise did — or didn't.

Asking someone to answer for judgment they were never positioned to see is not only an engineering mistake. It is a moral one. And it is one only the enterprise can correct, because no individual can build the review structures, authority boundaries, and escalation paths that make automated judgment governable.

## What ownership actually means

Not a guarantee of correctness. Organizations have never guaranteed that about human work either. They built structures that made imperfection governable and answered for the result.

Concretely, it means owning five decisions currently being made by default:

- what counts as a failure worth catching
- what the system is and is not permitted to do autonomously
- how often the system's behavior is checked against expectations
- what triggers human intervention, and who has authority to act
- how much human review exists, and where it is spent

These are not engineering settings. They are risk decisions, and they belong at the level that chose speed.

An enterprise that has not assigned these decisions to clear owners has still made them. It has simply made them by default.

---

## References

[1] Jacky Liang, *The Novelty Bottleneck: A Framework for Understanding Human Effort Scaling in AI-Assisted Work*, March 2026. A stylized model in which the fraction of a task requiring human judgment creates an irreducible serial component; better agents improve the coefficient on human effort but not the exponent. The author explicitly presents it as a framework rather than proof that human effort must scale linearly. <https://arxiv.org/abs/2603.27438>

[2] Faros Research, *The AI Productivity Paradox Report 2025*, analysis reflecting data through June 2025. Observational telemetry from more than 10,000 developers across 1,255 teams. Developers on high-AI-adoption teams completed 21% more tasks and merged 98% more pull requests, while PR review time was 91% longer. <https://www.faros.ai/blog/ai-software-engineering>

[3] LinearB, *2026 Software Engineering Benchmarks Report*, 2026. Analysis of 8.1 million pull requests across 4,800 engineering teams in 42 countries. AI-generated PRs waited 4.6× longer before review overall; fully agentic PRs waited 5.3× longer than unassisted PRs. Thirty-day acceptance rates were 32.7% for AI-generated PRs and 84.4% for unassisted PRs. <https://linearb.io/resources/software-engineering-benchmarks-report>

[4] Hao He, Courtney Miller, Shyam Agarwal, Christian Kästner, and Bogdan Vasilescu, *Speed at the Cost of Quality: How Cursor AI Increases Short-Term Velocity and Long-Term Complexity in Open-Source Projects*, MSR 2026. A matched, observational study of open-source GitHub projects. The authors' preferred estimator found a transient velocity increase alongside 30.3% more static analysis warnings and 41.6% greater code complexity; the quality estimates varied across alternative estimators. <https://arxiv.org/abs/2511.04427>

[5] McKinsey, *The state of AI in 2025: Agents, innovation, and transformation*, November 2025. Survey of 1,993 respondents; the negative-consequence question was put to the 1,753 whose organizations regularly used AI in at least one function. <https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-state-of-ai>

[6] McKinsey, *The state of AI: How organizations are rewiring to capture value*, March 2025. A separate survey, fielded July 2024, of 1,491 respondents across 101 nations. Twenty-eight percent said the CEO was responsible for overseeing AI governance, while respondents reported an average of two leaders jointly owning governance. <https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-state-of-ai-how-organizations-are-rewiring-to-capture-value>

[7] Schellman, *2026 State of AI Governance Report*, 29 July 2026. Survey of 525 U.S.-based professionals involved in AI governance at companies with at least 500 employees and $100 million in annual revenue. Forty-two percent named the CIO or head of IT as primarily responsible for purchasing; 37% named that role as ultimately accountable for AI risk. The aggregate results do not establish individual-level overlap. <https://www.schellman.com/blog/news/new-schellman-ai-research-report>

[8] Chee Hae Chung and Brian R. Spisak, *AI Cuts Costs, Until It Doesn't: Why Accountability Still Belongs to Senior Leadership*, California Management Review, 6 July 2026. A management commentary arguing that removing layers of human judgment can create short-term efficiency while eliminating buffers that once dispersed responsibility. <https://cmr.berkeley.edu/2026/07/ai-cuts-costs-until-it-doesn-t-why-accountability-still-belongs-to-senior-leadership/>

[9] Madeleine Clare Elish, *Moral Crumple Zones: Cautionary Tales in Human-Robot Interaction*, Engaging Science, Technology, and Society 5 (2019), 40–60. Introduces the moral-crumple-zone concept in automated and human–robot systems; its use here is an application by analogy to enterprise AI. <https://estsjournal.org/index.php/ests/article/view/260>

---

## Earlier in this series

- [The Models Are Getting Better. Code Still Cannot Fully Check Their Judgment.](Post 11)
- [The CPU Stack Runs on Specification. The LLM Stack Runs on Judgment.](Post 10)
- [AI Takes Duties, Not Accountability](Post 6)
- [AI Took Programming, Not Engineering. Will It Do the Same to Other Roles?](Post 4)
