# Does Software Ever Need to Call an Agent?

The last post ended with a question: where does traditional software genuinely need to call an agent, wait for its semantic judgment, and use that judgment to perform its own business function?

The question deserves a serious answer, because it decides something practical: how much of the agent story belongs *in the core* of an enterprise's systems, and how much belongs *around* it. But before the question can be answered, we need to see where reality actually sits.

Start with three familiar scenes. All of them get called, casually, "agent use cases":

- A research agent assembles a report. A human reads it.
- A customer service agent decides a refund is justified and calls `refund(order_id, amount)`.
- A CI/CD pipeline asks an agent, "Is this deployment safe?" — and must do something with the answer.

Same label. Three different destinations for the agent's judgment — and only one of the three is literally *software calling an agent*.

## Where does the judgment go next?

In the first scene (`Agent → Human`), the judgment ends at a human — the one participant built to consume open-ended semantic output. Code moved the data around; it never had to understand the report.

In the second (`Agent → Tool`), the agent owns the judgment. What crosses into software is not the judgment but its conclusion — a bounded operation the refund tool validates without interpreting. The label runs backward here: an agent using software, not software using an agent.

The third scene (`Code → Agent → Code`) is different in kind. Code invokes the agent. Code receives a human-like judgment. And code needs that judgment to decide what happens next: Code → Agent → (judgment) → Code.

Call this the **two-bill category**. Three conditions define it:

1. a code caller invokes the agent;
2. the code caller consumes the agent's human-like judgment;
3. that judgment is a necessary input for the rest of the business logic.

These labels describe *interactions*, not products. A single application can contain all three scenes at once. The question is always about one specific arrow — where does *this* judgment go next?

## Why "two bills"?

Why single this arrangement out? Because — put the last two posts together — it is the only one that pays both bills at once.

It pays the **speed bill**. A CPU-speed caller now depends on LLM-speed inference: seconds or minutes, variable, sometimes unavailable. The last post called this the operational mismatch — timeouts, retries, state, backpressure, fallback.

And it pays the **semantic bill**. When the answer arrives, the caller must act on it. The first post in this series named the difficulty: there is no cheap, deterministic check that a semantic judgment satisfies the original intent. `expected == actual` has nothing to compare against. Structure can be validated; the choice inside it cannot.

Every other arrangement offloads at least one of these bills. Agent → Human hands semantic consumption to the participant designed for it. Agent → Tool keeps the judgment inside the agent and hands code a precise operation. Only Code → Agent → Code asks deterministic software to wait for a slower participant *and then* act on an answer it cannot verify.

## If that is true, we can predict where the category lives

Take the two bills seriously and they tell you, in advance, where these systems will and won't be found.

A structure that pays an operational cost survives where that cost is small: latency-tolerant paths, asynchronous workflows, batch processing — call it the **slow lane**, in contrast to the fast lane of request paths, transactions, and control loops that run at the software's own speed.

A structure that pays a semantic cost survives where the judgment has been shrunk: answers constrained to categories the code already handles, wrapped in deterministic checks, with a human escape hatch nearby.

So if the two bills are real, we should expect genuine Code → Agent → Code cases to cluster in the cheap corner: **the slow lane, with the judgment narrowed as far as it will go**. And the fuller the judgment or the faster the path, the rarer the sightings should get.

## Checking against reality

A recent study looked at 6,003 publicly available n8n workflows with LLM components [1]. Across 9,773 three-node neighborhoods centered on an LLM, the most common pattern was logic/control → LLM → logic/control. Code-like machinery on both sides of a model, by the thousands.

That confirms the shape, not the category. A static graph cannot show what kind of judgment crossed the LLM boundary, whether downstream logic needed its meaning, or what happened at runtime. The study does not inspect all 6,003 workflows against our three conditions.

But it gives us some low-hanging cases to examine, and those fit the prediction.

In the study's invoice case, a model extracts fields; code repairs the JSON, detects duplicates, and matches records. The authors are explicit about the limit: those checks catch malformed, inconsistent, or duplicate output without proving the extraction semantically correct, and ambiguous cases go to a human. The judgment has been narrowed enough for code to contain and route it, while the deepest semantic check remains outside deterministic software. None of this is a new arrangement, either — machine learning did this job, with weaker engines, for years before anyone said "agent."

The task mix points in the same direction, although it cannot prove the destination. Text generation — the largest category — includes drafts, messages, and documents naturally addressed to human readers. Planning or agentic execution — about a sixth of the workflows — includes the agent-owned-loop pattern in which agents call tools. These are clues, not a census: the task labels are not cross-tabulated with judgment consumers, and neither category maps cleanly onto our three arrangements.

The paper also says its evidence comes from static workflow definitions, not runtime behavior, and that public workflows skew toward templates and demos. It cannot show that the two-bill category is empty. It can only make that hypothesis more or less plausible.

So here is my guess: most apparent Code → Agent → Code cases take one of three exits before paying both bills in full. The judgment is narrowed before code uses it, the open-ended output is addressed to a human, or the agent owns the loop and calls bounded tools. If that is right, genuine two-bill instances are rare and may be absent from ordinary enterprise software. That is my reading of a narrow public window, not the study's finding.

## What the evidence does not settle

Software calls LLMs mid-execution every day. What remains unconfirmed is the specific arrangement we have been looking for: traditional code calling an agent, consuming its human-like judgment, and needing that judgment to continue its own business logic.

The evidence gives us a reason to suspect that arrangement is rare. It does not settle the question in the title, and it does not tell an enterprise what to do with the uncertainty.

For that, the hypothesis needs a map.

## The map

Put the two bills on axes and the landscape becomes visible. One axis is operational: how much LLM-speed the caller can tolerate — the **slow lane** of workflows and batch processing versus the **fast lane** of request paths, transactions, and control loops that run at the software's own speed. The other axis is semantic: how narrowed the judgment is that the caller consumes. The slow lane makes the speed bill easier to carry; narrowing makes the semantic bill easier to measure and contain.

```text
                      judgment: narrowed          judgment: open-ended

  slow lane           familiar territory —        my guess: rare;
  (latency-tolerant)  extraction, routing,        often human-bound
                      gates, classification

  fast lane           established territory —     highest-cost corner;
  (CPU-speed paths)   classical ML                poor default bet
```

These cells are not measured populations. They are a decision map built from the pressures the two bills create. Three of them reward a closer look.

**Slow × open-ended** is where my guess needs the most care. In the low-hanging cases, genuinely open-ended output — an analysis, an assessment, a recommendation — is commonly addressed to a person: a draft to review, a report to read. At that point it is Agent → Human wearing workflow clothes, and the semantic bill has been transferred to the participant who can pay it. That does not prove the quadrant empty. It tells us what to inspect first: who consumes the judgment?

**Fast × narrowed** is the quadrant the agent conversation keeps forgetting. Machine-speed judgment inside business logic is not new. Fraud scores, risk models, recommendation engines, and price optimization have run in the fast lane for years. They earned admission by shrinking judgment until performance became measurable across cases — a score, a class, an error rate you can put on a dashboard and argue about in a review. Evaluation does not prove each decision correct, but it makes the risk governable.

**Fast × open-ended** is the corner where both bills are largest. Calling it a poor bet as the default is an architectural decision, not an empirical claim that the cell is empty: in the limited evidence I examined, I did not identify a clear case showing why an ordinary enterprise needs this arrangement enough to accept both costs.

## Why we would not build there by default

In the slow lane, time can buy containment: retries, downstream checks, reconciliation, a human who can be asked. The patterns I examined use some combination of those options.

At CPU speed, fewer safeguards fit before the consequence. Code may still enforce deterministic limits or make the action reversible, but those measures contain the impact; they do not verify the open-ended judgment itself.

The quadrant is not forbidden, and it is not permanent desert. It becomes easier to justify when verification catches up, when consequences can be bounded or reversed, or when the value is compelling enough to accept the residual uncertainty. The first is a research frontier. The others are engineering and governance decisions.

There is also an incumbent to displace. Open-ended agent judgment does not enter an empty fast lane; it competes with narrow models whose behavior can be measured. To win, it would have to prove it is *better enough to excuse being harder to verify*. That is a high bar. The evidence I examined did not give me a documented ordinary-enterprise case that clearly clears it — an invitation for a counterexample, not proof that none exists.

## What this means for AI transformation

For a bank, a retailer, an insurer — an enterprise whose business is not building models — the map is a decision rule, not an inventory of everything deployed.

The better-supported opportunities route around the hardest version of the problem. That is not a diminished version of the agent story. It is the low-regret place to start.

My recommendation is not to put open-ended agent judgment in the fast path of the business by default. Treat it as a frontier to *watch*, not a milestone to schedule, until a specific use case makes both bills worth paying.

## Ruling out before building

Why spend a whole article reaching "not yet"?

Because ruling out for a roadmap is engineering. An architect does not need to prove that a design is impossible before excluding it from this build. When its costs are clear, its necessity is unconfirmed, and better-supported alternatives exist, "no, not yet" is a sound decision. It clears the design from today's commitment without ruling it out forever. An enterprise that knows what it is *not* building can put its weight behind what it is.

AI-transformation roadmaps often skip this step. They inherit their priorities from what demos well, and the gap can show up later — as pilots that stall when the pilot's slow lane meets the production system's fast one.

## What this leaves standing

Here is what that decision leaves standing. The same lens directs attention to three better-supported arrangements:

- agents producing work for humans to consume;
- agents owning decisions and calling software as bounded tools;
- slow-lane calls that return narrowed judgment wrapped in checks.

These are not the leftovers. Each avoids or contains at least one bill, and each corresponds to concrete patterns examined above. They are arrangements enterprises can evaluate, build, and govern with tools they already understand. The map that says "not now" to the highest-cost corner is an itinerary for the better-supported ground.

That is where this series goes next: into those better-supported arrangements, asking what each one is genuinely good for in the transformation of a regular enterprise — a bank, a retailer, an insurer — rather than an AI vendor.

The lens comes along unchanged. For every use case that follows, the same three questions get asked first: at what speed does it run, where does the judgment go next, and how narrow is that judgment? Answer those, and a use-case label — "customer service agent," "research assistant," "automation" — turns into an architecture you can actually evaluate.

## The question stays open

The map is a hypothesis and a decision rule, not a census. So the question in the title remains genuinely open, and it is now precise enough to answer:

> **Can you name a real system where code — not a human, and not the agent itself — must consume an agent's human-like judgment as a necessary input to its own business function? How open-ended is the judgment, how fast must the path run, and what makes both bills worth paying?**

If you have one, I want to see it. It would be the most interesting counterexample this series could get.

Ruled out for this roadmap, not ruled out of existence. Now we build on the ground that holds.

---

## References

[1] Yutian Tang, Yuming Zhou, and Huaming Chen, *Characterizing Large Language Model Agentic Workflows: A Study on N8n Ecosystem*, 2026 (preprint, arXiv:2606.29116). The study analyzes 6,003 publicly available n8n workflows containing LLM components; `logic/control → LLM → logic/control` is the most common pattern among 9,773 LLM-centered three-node neighborhoods (27.11%), and the leading primary tasks are text generation (31.12%), information extraction (18.34%), and planning or agentic execution (16.64%). The authors caution that static workflow structure cannot establish runtime behavior, and that public workflows skew toward templates and demonstrations.
