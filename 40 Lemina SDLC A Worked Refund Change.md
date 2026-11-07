# 40. Lemina SDLC: A Worked Refund Change

*Originally published on LinkedIn: <https://lnkd.in/p/gDdBRvYG>*

## 1. From the proposal to a worked example

Post 39 introduced Lemina SDLC as a structural guideline: organize software into persistent micro-features, arrange them in a tree, and bring each one's artifacts together as a versioned set.

Lemina has evolved through these posts. Post 38 described human-held roles and handoffs as part of Lemina; under Post 39's definition, those choices belong to the surrounding method.

Post 38 also promised an evaluation through its four lenses, a comparison with some of the cases, and a look at Lemina's bet on small reviews, and Post 39 moved that evaluation here. I've dropped that plan. The lenses were built for whole methods, and I have no established way to evaluate only the structural half of one. This post is a worked example instead: it applies the structure to Post 36's refund-policy change to show how Lemina arranges the artifacts.

The retailer, team, artifacts, and comparisons below are invented, and nothing was run. The example shows where the rules go, what the parent and children hold, and how their artifact sets relate. A later checkout change returns to the records left by the refund change.

**The worked example makes the artifact arrangement concrete.**

## 2. A team applies Lemina

### 2.1 Giving the refund rules a place

A retailer changes its refund policy. Today, customers get a full refund within 30 days of delivery. From March 1, they still get a full refund within 30 days, but from day 31 to day 60 they get store credit instead. Defective items can be refunded for 90 days, final-sale items can't be returned, and orders placed before March 1 keep the old rule. A refund over $500, or a customer's third refund in 12 months, goes to a person for review.

The starting structure is Post 39's example (§2.3): refund processing is a parent with three children, eligibility assessment, amount calculation, and refund recording. This team adds a fourth child, review routing, to hold the review thresholds and the refund-history lookup. Those rules could sit in the parent instead. The team chooses a separate child so that its requirements, specification, design, code, and tests can be read together.

For concreteness, the team treats the systems involved as parts of one project. It uses folders in one Git repository, Markdown documents with labeled rules, and Java with JUnit for code and tests. It records sources for business clarifications and uses "Depends on," "Used by," and "Constrained by" lines for relationships. These are the team's conventions. Lemina supplies the tree and versioned artifact sets; the team chooses their boundaries, contents, layout, and formats. The boundaries are guided by the size of the tasks and context AI must handle.

Here is the relevant part of the tree after the change. Eligibility assessment is expanded; the other children have the same artifact types. Artifacts outside refund processing are omitted.

```text
retail-platform/                         root micro-feature
├── refund-processing/
│   ├── requirements.md                  combined behavior and shared constraints
│   ├── specification.md                 behavior of the whole refund flow
│   ├── design.md                        how the children fit together
│   ├── src/RefundFlow.java              integration code
│   ├── tests/RefundFlowTest.java        tests of the combination
│   ├── eligibility-assessment/
│   │   ├── requirements.md
│   │   ├── specification.md
│   │   ├── design.md
│   │   ├── src/Eligibility.java
│   │   └── tests/EligibilityTest.java
│   ├── amount-calculation/              requirements, specification, design, code, tests
│   ├── review-routing/                  requirements, specification, design, code, tests
│   └── refund-recording/                requirements, specification, design, code, tests
├── store-credit-ledger/
├── finance-reporting/
├── checkout/
├── order-system/
├── returns-service/
├── payments/
└── customer-service-tools/
```

The nested folders show the hierarchy. They do not make the children's artifacts part of the parent's set. Refund processing holds its own five artifacts; eligibility assessment holds its five, and likewise for each sibling. As Post 39 puts it, "A parent organizes its descendants rather than containing them" (§2.3).

The policy is distributed according to what each part does:

| Rule or behavior | Where it belongs in this example | State for this change |
|---|---|---|
| Time windows, defective and final-sale items, and the pre-March rule | Eligibility assessment | Changed |
| The amount for each returned item | Amount calculation | Entire set unchanged; its calculation does not depend on the form |
| Review over $500 or on a customer's third refund in 12 months | Review routing | New |
| Issuing cash or store credit and supplying refund history | Refund recording | Changed |
| Shared constraints and the combined behavior | Refund processing | Changed |
| Store-credit entries and financial reporting | Store-credit ledger and finance reporting | Changing separately, outside the refund-processing pull request |

The order system still supplies dates, the returns service supplies the defective flag, payments handles cash refunds, and the customer-service tools hold pending cases. Their specifications remain relevant even where their artifacts do not change. The ledger serves other functions too, so the team keeps it in another subtree. The dependency crosses the tree. These boundaries imply no separate deployment units.

**One business change spans several micro-features, including some whose artifacts stay unchanged.**

### 2.2 The parent's own artifacts

To decide what belongs in the parent, the team uses a rule of thumb: put a rule there if it binds more than one child or concerns how the children fit together; otherwise put it in the child that can satisfy it alone. The first half follows Post 39 (§2.3); applying it to each rule is the team's design judgment. The parent therefore leaves eligibility's rules, the day arithmetic, and the $500 threshold to its children; its integration tests exercise them.

The following excerpts show the settled state after the clarifications in Section 2.5. The parent's requirements include:

```text
P1. A refund never exceeds the original payment.
P2. "Refund" includes store credit. Items returned together count as one refund.
P3. A return requiring review waits for a person's decision.
    While pending, it produces no cash refund, store credit, or completed refund record.

Source for P1: existing refund policy.
Source for P2: refund policy owner, clarification of form and refund unit.
Source for P3: refund policy owner, clarification of review behavior.
```

P1 was already there, and its wording stays the same. P2 brings store credit under P1 and both review triggers, and defines the unit those triggers concern. Amount calculation's specification already holds each item's amount to what was paid for it, so that set stays unchanged.

The parent's specification describes the combined behavior:

```text
For each item, determine whether it is returnable and, if so, its form and amount.
If no item is returnable, refuse the return and issue nothing.
Route the return once, using the total amount for its returnable items.
A return requiring review remains pending until a person decides.
For a cleared or approved return, issue the returnable items in their permitted forms
and record one completed refund for the return. A refused review produces no refund.

Depends on: customer-service-tools (pending cases)
Used by: returns-service; customer-service-tools
```

One return can contain different forms: on day 45, a plain item receives store credit while a defective item receives a cash refund. The parent preserves those per-item outcomes while applying the review rules to the return.

Its design explains the composition:

```text
For each returned item, assess eligibility.
If returnable, calculate its amount; otherwise exclude it from issuance and the total.
If nothing is returnable, finish without a refund.
Route the return once, on the total of the calculated amounts.
If it needs a person's decision, retain a pending case in the customer-service tools.
After clearance or approval, pass one return identifier and the returnable items'
forms and amounts to refund recording. Do not call issuance while a case is pending.
```

The team puts the connecting code in the parent. Post 39 also permits integration code in an appropriate leaf (§2.3); this example chooses the parent because the code implements the combination described here.

The parent's tests exercise that combination. For example:

```java
@Test void storeCreditOver500WaitsForReview() {
    Case c = flow.process(returnOf(newOrder, itemPaid(usd(501)), delivered.plusDays(45)));
    assertEquals(WAITING_FOR_REVIEW, c.status());
    assertTrue(payments.refunds().isEmpty());
    assertTrue(ledger.entries().isEmpty());
}
```

Other flow tests cover a customer's third refund in 12 months, three $200 items triggering review on their $600 total, and a completed multi-item return counting once in history. A mixed return exercises both cash and store credit while retaining one refund record.

A shared constraint thus appears in three places: it is stated once in the parent's requirements, named in each affected child's specification, and exercised in parent tests. Lemina places shared constraints with the parent; the reference lines are this team's convention.

The processing order above is runtime behavior of the retailer's software. The order in which the team writes and reviews these artifacts is a choice of its surrounding method.

**The parent holds the shared constraints, the combination, and tests of that combination.**

### 2.3 Inside eligibility assessment

Eligibility assessment is a leaf. Its set holds the implementation along with the documents describing it. The excerpts are shortened.

In the settled requirements, the refund policy owner has confirmed that the delivery date is day 0 and that the defective-item exception takes precedence over final sale:

```text
R1. Days count from delivery; the delivery date is day 0.

For orders placed on or after March 1:
R2. Returned by day 30: full refund, subject to R6.
R3. Returned from day 31 to day 60: store credit, subject to R5 and R6.
R4. Returned after day 60: not returnable, unless R5 applies.
R5. A defective item gets a full refund through day 90.
R6. A final-sale item can't be returned, unless R5 applies.

For orders placed before March 1:
R7. The previous rule applies: a full refund within 30 days of delivery.

Source: March refund policy; refund policy owner for day numbering
and the precedence of R5 over R6.
```

The specification describes what other parts can rely on:

```text
Input:
  Order placed and delivered dates; final-sale and defective flags; return date.
  Dates have been validated; return is not before delivery.

Output:
  FULL_REFUND, STORE_CREDIT, or NOT_RETURNABLE. No amount or review decision.

Outcomes:
  Orders before March 1: FULL_REFUND through day 30, then NOT_RETURNABLE.
  Orders from March 1: defective, FULL_REFUND through day 90, even if final sale;
  other final-sale items, NOT_RETURNABLE; otherwise FULL_REFUND through day 30,
  STORE_CREDIT from day 31 through day 60, then NOT_RETURNABLE.
  The delivery date is day 0.

Depends on: order-system (dates); returns-service (defective flag)
Used by: refund-processing
```

The design records the approach and why:

```text
Use one function with no stored state, deciding from supplied dates and flags.
Keep payment, ledger writes, amount calculation, and refund history outside this leaf.
Use calendar-date differences for R1.
Select the old-policy branch first.
For newer orders, test the defective exception before final sale, then the ordinary windows.
The first matching rule decides.
Rejected: a rules engine. A handful of rules that rarely change don't need one.
```

Here is the corresponding code. POLICY_START names March 1 in the retailer's agreed policy year.

```java
Outcome assess(Order order, Item item, LocalDate returned) {
    long day = DAYS.between(order.deliveredOn(), returned);
    if (order.placedOn().isBefore(POLICY_START))
        return day <= 30 ? FULL_REFUND : NOT_RETURNABLE;
    if (item.isDefective() && day <= 90) return FULL_REFUND;
    if (item.isFinalSale())              return NOT_RETURNABLE;
    if (day <= 30)                       return FULL_REFUND;
    if (day <= 60)                       return STORE_CREDIT;
    return NOT_RETURNABLE;
}
```

Selected tests express the boundary cases and exceptions:

```java
@Test void day30IsFullRefund() {
    assertEquals(FULL_REFUND, assess(newOrder, plainItem, delivered.plusDays(30)));
}
@Test void day31IsStoreCredit() {
    assertEquals(STORE_CREDIT, assess(newOrder, plainItem, delivered.plusDays(31)));
}
@Test void defectiveOnDay75IsRefunded() {
    assertEquals(FULL_REFUND, assess(newOrder, defectiveItem, delivered.plusDays(75)));
}
@Test void finalSaleIsNotReturnable() {
    assertEquals(NOT_RETURNABLE, assess(newOrder, finalSaleItem, delivered.plusDays(5)));
}
@Test void defectiveFinalSaleOnDay75IsRefunded() {
    assertEquals(FULL_REFUND, assess(newOrder, defectiveFinalSaleItem, delivered.plusDays(75)));
}
@Test void oldOrderOnDay31IsNotReturnable() {
    assertEquals(NOT_RETURNABLE, assess(oldOrder, plainItem, delivered.plusDays(31)));
}
```

R3, the specification's outcomes, the code's day-60 branch, and the day-31 test state one rule at different levels, for different readers. The design explains how the implementation applies those rules. All five artifact types belong to the same set.

**Requirements, specification, design, code, and tests describe and implement one contribution together.**

### 2.4 Understanding the whole through the children

The siblings have their own full sets. Here are the parts of their specifications needed to understand the parent. T1 and T2 are review routing's requirements for the amount and third-refund triggers:

```text
amount-calculation (entire set unchanged)
Input:          one returned item and the order it came from
Output:         the amount for that item: what the customer paid for it, never more
Constrained by: refund-processing P1
Used by:        refund-processing

review-routing (new)
Input:          the return's total amount; return date; customer's completed refund history
Output:         REVIEW if the total is over $500 (T1) or this would be the customer's
                third refund in 12 months (T2); otherwise PROCEED
Depends on:     refund-recording (one completed refund per return, with its date)
Constrained by: refund-processing P2
Used by:        refund-processing

refund-recording (changed)
Input:          one return identifier, its returnable items' forms and amounts,
                and clearance or approval
Behavior:       issue each item through payments or the store-credit ledger as its form requires
Output:         one completed refund record for the return, with dated item lines
                showing their forms and amounts
Provides:       refund history counting each completed return once, across both forms
Depends on:     payments (cash refunds); store-credit-ledger (refund-credit entries)
Constrained by: refund-processing P1, P2, P3
Used by:        refund-processing; review-routing; finance-reporting
```

With these, eligibility's specification, and the relevant documents in the parent's set, a reader can follow a case without opening Java. Take one $600 ordinary item returned on day 45 from an order placed after March 1:

1. Eligibility's specification gives STORE_CREDIT.
2. Amount calculation's gives $600, the amount paid.
3. Review routing's gives REVIEW because the amount is over $500; P2 includes store credit.
4. P3 and the parent's design hold the case. Refund recording is reached only after approval.

This is the composition Post 39 describes: the children's specifications stand in for their implementation detail. A question about how the amount is calculated leads into that child's own set. The route holds only while the specifications match the implementations they describe.

The relationship lines show a child bound by its parent, a sibling using another sibling's history, and dependencies across subtrees. Until the ledger's change is accepted, the refund change relies on an entry type the ledger's current specification does not offer. Finance reporting also needs its separate change to distinguish the recorded forms. The team arranges compatible versions for release through its delivery method.

There are two reading paths. To examine refund processing as a whole, an actor starts with the parent's set and its children's specifications. To work on one child, an actor starts with that child's full set, the relevant parent requirements, and specifications it depends on. As Post 39 says (§2.4), "The full set is not the context": what prepares an agent's context still selects what that task needs.

**The parent's artifacts and the children's specifications explain the combination; each child's full set supplies the detail.**

### 2.5 Examining an early candidate

The team proposes the refund-processing changes in one pull request. Its surrounding method assigns policy decisions to the refund policy owner, implementation review to a code reviewer, and release approval to a release approver. People or agents may draft, code, and compare artifacts. The team uses CI for compilation and tests and a review agent to compare relevant artifacts. Those assignments, approvals, tools, and the sequence of work are team choices.

Post 39 (§2.2) calls the latest accepted state the current version. Applying proposed edits to it produces a candidate version. The excerpts above show the settled result; selected parts of an earlier candidate looked like this:

| Artifact | State in the early candidate | Why relevant |
|---|---|---|
| Eligibility assessment's set | Changed, with final-sale precedence still unsettled | It describes and implements the new policy |
| Review routing's set | New | It applies the two review triggers |
| Refund recording's set | Changed | It issues store credit and supplies history |
| The parent's P2, P3, specification, design, flow code, and flow tests | Changed | They describe the new shared rules and combination; the refund unit is not yet explicit |
| The parent's P1 | Unchanged | Its payment cap still applies |
| Amount calculation's entire set | Unchanged | Its per-item amount feeds the parent's flow; its specification describes that unit |
| Ledger, reporting, and customer-service tools' specifications | Unchanged by this pull request | They describe the external relationships |

The candidate is the resulting state, not just the edits. Examining it means comparing relevant artifacts as they will stand together, including unchanged ones. It does not mean rechecking the whole application on every change.

The team happened to start with parent requirements, work down to the children's code, and return to the parent tests. Another team could start from a failing test, draft several artifacts together, or use several pull requests. Lemina prescribes none of these orders. A method that also writes documents for one change would still need to carry what they establish into the persistent sets.

In this illustration, the review agent reports two comparisons that lead from the early candidate to the settled excerpts above.

**Within eligibility's set.** R5 and R6 did not say which wins for a defective final-sale item. The design and code nevertheless tested final sale first. The policy owner decides that the defective exception wins. That clarification reaches all five artifact types:

- Requirements: R6 gains "unless R5 applies," with the decision's source.
- Specification: the outcomes explicitly permit a defective final-sale refund through day 90.
- Design: the defective check precedes final sale.
- Code: the conditions follow that order.
- Tests: the defective final-sale case is added.

**Against a correct unchanged set.** The early parent design routed and recorded each item separately. Review routing's T1 concerned a refund over $500. Amount calculation's unchanged specification said its output was for one item. Together, they showed that three $200 items would not trigger T1, despite a $600 total. Per-item recording would also count that return three times in history.

The policy owner confirms that items returned together count as one refund. P2 records that decision. The parent now totals the returnable amounts and routes once. Recording groups the item lines under one return, and its history counts that return once. Review routing's specification names the total and that history unit; the affected designs, code, and tests follow. Amount calculation's set stays as it was, correct and unchanged. Its specification confirms that the amounts are per item.

In this example, the grouping tells the team what to put side by side: eligibility's rule, design, and code sit in one set, and the parent's design leads from the routing rule to an unchanged specification the diff doesn't show. The business decisions come from the policy owner; the comparisons expose where they are needed.

Acceptance makes the new versions current; amount calculation's set keeps the version it had. "Current" means latest accepted, not necessarily running: as in Post 36, the team releases behind a feature flag, one region at a time.

**Examining a candidate brings changed artifacts together with the relevant unchanged artifacts they must agree with.**

### 2.6 Returning for the checkout change

After the policy goes live, support sees more tickets about day-31 store credit. As in Post 36, a backlog item follows: clarify the message at checkout.

The team adds a return-policy notice as a leaf under checkout. Its set holds requirements, specification, design, rendering code, and tests. Support tickets explain why the work is needed. Eligibility's current requirements and specification supply the policy it must describe.

```text
checkout/return-policy-notice/requirements.md
N1. For ordinary items, state the windows in eligibility-assessment R1–R4
    and the defective exception R5.
N2. For final-sale items, state the restriction and exception in R5–R6.
Source: support tickets; eligibility-assessment requirements.

checkout/return-policy-notice/specification.md
Depends on: eligibility-assessment (R1–R6 and outcomes)

refund-processing/eligibility-assessment/specification.md
Used by: refund-processing; checkout/return-policy-notice
```

Checkout now takes orders placed after March 1, so R7's grandfathered orders do not belong in the notice. The ordinary-item message reads: "Return within 30 days of delivery for a full refund. From day 31 to day 60, you'll get store credit. Defective items can be refunded within 90 days of delivery." Tests cover those windows and the final-sale variant.

This candidate contains the new notice set, an added entry on eligibility's "Used by" line, and the unchanged policy artifacts relevant to the notice. Eligibility's set gets a new version even though only its specification changes. Versioning a set as a unit does not mean editing every artifact. The reverse link also names the notice as a dependent to consider when a window changes.

Someone working on checkout, possibly in another agent call and without the first conversation, can return to the durable rule and its source.

A mistake would travel the same route. If R1 said "from purchase," and the specification, code, and tests followed it, their agreement would not establish the intended rule. The notice would repeat the mistake. Detecting that mistake takes evidence of business intent from outside the agreeing artifacts.

**Persistence lets later work return to a decision instead of re-deriving it, whether the decision was sound or not.**

## 3. The arrangement the example leaves behind

The refund policy has places in the software's structure. Eligibility holds the windows and exceptions. Amount calculation holds the per-item amount behavior. Review routing holds the review triggers. Recording holds issuance and refund history. Their parent holds the shared constraints, combined behavior, and tests of the whole. Other subtrees keep their own artifacts and describe the relationships between them.

A proposed change produces a candidate version: the state that results from applying its edits. Examining it includes the relevant unchanged artifacts. Accepted sets persist for subsequent work. Specifications carry a reader through their contributions to the parent, with each full set available for deeper work.

This invented example does not establish effectiveness, reduced review effort, or the digestibility target of doing a role's task on a micro-feature within one agent call. Those require assessment with actual tasks, models, and agents.

**Lemina gives functionality lasting spaces and versioned artifact sets; the specifications in those sets are the route to understanding their contributions together.**

---

## Earlier in this series

All posts are archived in the GitHub repository: <https://github.com/young-rex/notes-on-agentic-ai>

- Post 39: *Lemina SDLC: My Own AI-Native Proposal* <https://lnkd.in/p/gR-Uu5hA>
- Post 38: *Evaluating AI-Native SDLC: Four Cases Through the Same Four Lenses* <https://lnkd.in/p/gRgZVnD4>
- Post 36: *The SDLC We Already Practice, Before AI* <https://lnkd.in/p/gtYC_M4g>
