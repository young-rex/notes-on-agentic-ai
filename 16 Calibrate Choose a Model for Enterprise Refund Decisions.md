# Calibrate: Choose a Model for Enterprise Refund Decisions

Take one feature an enterprise might build, identify the mechanisms it requires, locate each one, and see what the locations tell you about choosing a model.

Post 14 mapped agentic AI as a five-phase arc. Post 15 asked the more useful question: where does each capability live — in the model's weights, the harness around it, or both? That produced a broad table and a principle: put judgment in the weights, put guarantees in the harness. But the table was broad, not directed. This post directs it — a worked reading of Post 15's map against a single enterprise feature.

## The feature

An enterprise wants an agent to decide whether a customer's case justifies a refund: approve, deny, or escalate. Deciding is the model's job; checking that the decision is permitted, and executing it, belong to deterministic software. Keeping those responsibilities separate is part of the method.

The enterprise's question: which model should make the decision?

## Three mechanisms, three locations

Most of what a production refund system requires — context assembly, tool registries, observability, retry logic — is infrastructure around the feature, not the feature itself. The question is: what mechanisms does this feature require, and where does each one live?

Three mechanisms. One in each location.

| Mechanism | Location | What it provides |
|---|---|---|
| Instruction following | Weights | The model applies learned judgment to the case and the policy |
| Structured outputs | Hybrid | The decision arrives in a format code can consume and the model can populate meaningfully |
| Permissions | Harness | The enterprise's limits on what the AI may do |

Each produces a different kind of evidence. Confusing them is where model comparisons go wrong.

## Where capability lives determines the evidence it owes

Post 15's map is not a product catalog. It is a way to know what questions to ask and what answers to accept.

### Weights: demand behavioral evidence

Instruction following is learned behavior. The enterprise can improve the instructions, supply examples, retrieve the relevant policy, choose a reasoning mode. Those interventions influence the result. None turns policy adherence into a deterministic guarantee.

So a vendor's claim that a model "supports instruction following" answers the wrong question. The right one is: *on our cases, how often does this deployed configuration make the decision our policy requires?*

That question requires evaluation on representative business cases — including incomplete claims, conflicting policy clauses, borderline amounts, and attempts to override policy. A wrongful refund and an unnecessary escalation have different consequences and deserve separate counts.

IFBench tested whether models generalize to 58 unseen, verifiable instructions and found substantial variation — strong models struggle to generalize beyond the constraints they trained on [1]. Feature support is an admission ticket. It is not fitness for a duty.

### Hybrid: validate both halves

Require the decision to arrive as structured data: `{"action": "refund", "amount": 100, "policy_clause": "R-4"}`.

Two things happen. The runtime constrains generation to a schema — that is a real guarantee at the boundary. And the model populates the schema meaningfully — that is judgment. Post 11 named the limit: verified shape, not verified judgment. The structure proves `"refund"` is a valid value. It does not prove that refunding was right.

A 2026 evaluation of 21 models quantifies the gap: schema compliance was near-perfect, while the best exact value accuracy reached 83.0% on text and fell sharply on other modalities [2]. The structure was right. The values inside it were often wrong.

A structured-output evaluation therefore needs two scorecards measured separately — contract performance and semantic performance. Testing only JSON validity leaves the business decision unmeasured.

### Harness: require enforcement, not comparison

The agent may conclude that a refund is justified. Whether it has authority to issue one is a different question.

Refund ceilings, approval thresholds, rate limits, and prohibited actions belong in deterministic software around the model. Permissions do not need a comparative model benchmark. They need explicit rules, negative tests, and proof that every execution path passes through the enforcement point.

A prompt saying "never refund more than this amount" may remain useful guidance. It is not the control.

The short rule across all three:

> **Select what is learned. Validate what is shared. Enforce what must be guaranteed.**

## How to choose

The three locations tell you what evidence to demand. The decision rule tells you what to do with it.

Set minimum thresholds for policy accuracy, wrongful-action rate, missed-escalation rate, P95 end-to-end latency, and throughput — before seeing results. Among configurations that pass every gate, choose the lowest cost per safely completed case, counting inference, retries, and human review.

The selection unit is not a model name. Production behavior comes from a configuration: model and pinned version, reasoning mode, system instructions, schema enforcement, provider, and service tier. Changing any of these can change accuracy, latency, cost, or failure behavior. The unit being qualified is the deployed configuration — and a model upgrade or material runtime change triggers requalification.

For short structured responses, P95 end-to-end latency is more informative than output-token speed. A short refund decision may spend more time waiting for the first token than emitting its small JSON object. Throughput remains a separate capacity requirement.

## What this exercise shows

Three mechanisms, one per column, each demanding different evidence. A decision rule derived from the locations, not from a generic vendor scorecard. The illustration is specific. The method generalizes: identify the mechanisms a feature requires, locate each one, and let the location determine what evidence to demand.

Two observations stay with me.

The first is what model selection does and does not address. It addresses judgment performance — how well this configuration applies the policy to the case. It does not address authority, accountability, or containment. Those are harness decisions the enterprise makes regardless of which model it picks. An enterprise that selects the best-performing model and stops there has answered the Weights column and left the Harness column unaddressed.

The second is the ratio. The enterprise thinks it is asking one question: which model should we choose? The map reveals three decisions. Select the learned judgment in the weights. Qualify the contract shared by model and runtime. Design and enforce authority in the harness.

All three decisions belong to the enterprise. Only the first can be answered by comparing models. A better model may improve the first and contribute to the second. It can never supply the third.

Choosing the model is a real decision, and it rewards careful evaluation. It is not the whole of the feature.

---

## References

[1] Valentina Pyatkin et al., *Generalizing Verifiable Instruction Following*, NeurIPS 2025. Introduces IFBench, covering 58 unseen verifiable constraints, and finds that strong models struggle to generalize precise instruction following beyond familiar benchmarks. <https://arxiv.org/abs/2507.02833>

[2] Abhinav Kumar Singh, Harsha Vardhan Khurdula, Yoeven D. Khemlani, and Vineet Agarwal, *The Structured Output Benchmark: A Multi-Source Benchmark for Evaluating Structured Output Quality in Large Language Models*, 2026 preprint. Evaluates 21 models; reports schema compliance separately from exact value accuracy. <https://arxiv.org/abs/2604.25359>

---

## Earlier in this series

- [Calibrate: Where Does the Capability Live?](Post 15)
- [Agentic AI Moves Too Fast. Calibrate.](Post 14)
- [The Models Are Getting Better. Code Still Cannot Fully Check Their Judgment.](Post 11)
- [Write Software That Uses Agentic AI](Post 1)
