## Agents Break Open Software's Stable Relationship

*Originally published on LinkedIn: <https://lnkd.in/p/gVtniMsP>*

Software has changed constantly, but one relationship underneath it remained remarkably stable.

Humans expressed intent through code. CPUs executed that code.

Human intent ──expressed as──▶ Code; Code ──executed by──▶ CPU

The reality was always richer than this picture. Many human roles contributed intent, requirements, design, implementation, testing, and approval. Humans also interacted with running software through interfaces and operational workflows.

But inside the software's control path, the basic relationship held: code told the machine what to do. When a precise specification existed, the result could be checked mechanically. Decades of engineering practices grew around this arrangement.

Agents break that relationship open.

They do not replace it. Agent-generated code still runs through deterministic software and hardware. An agent's tool call is still executed by conventional code. The stable relationship remains underneath.

What changes is that it no longer describes the whole system.

Agents are a new kind of participant, with an unusual combination of properties. They can interpret language, context, ambiguity, and intent, but they can also be called during execution. They can write code, call code, or be called by code.

The combination comes from the LLM underneath: semantic capability plus machine callability. The system becomes agentic when the model's judgment starts to influence what happens next—the process, the tool call, the next action.

That creates new relationships before we have settled the engineering contracts that govern them.

## Different native speeds

The participants do not operate at the same characteristic pace.

Deterministic code operates at CPU-speed. Individual operations commonly complete quickly enough to compose into request paths, transactions, and control loops.

Model inference operates at LLM-speed. A response may take seconds. An agentic task that plans, uses tools, observes results, and revises its approach may take minutes or longer. Its duration is also more variable.

Humans operate at another pace again. They may respond in minutes, hours, or days—or be unavailable entirely.

These are their **native speeds**. When one participant depends on another operating at a different native speed, the system crosses a **speed boundary**.

That boundary is not merely a performance concern. It changes the shape of the integration. A faster participant should not block casually on a slower one.

Choosing to wait anyway requires decisions about timeouts, queues, retries, state, backpressure, caching, and fallback behavior. If a human is involved, it may also require durable pause and resume, notification, expiration, and an audit trail.

The direction of the relationship therefore matters. A slow participant calling a fast one creates a different system from a fast participant calling a slow one.

## Different kinds of processing

Speed is only half of the difference.

Code executes precise instructions. When we can state an expected property precisely, software can often check it mechanically: expected == actual.

Agents perform semantic inference. They choose what a request probably means, which context matters, what plan to follow, or which action fits the situation.

Their output can be structured: { "route": "refund" }.

But valid structure proves only that "refund" is an allowed value. It does not prove that issuing a refund satisfies the original intent.

This creates a second boundary: a **semantic boundary**.

The speed boundary creates an **operational mismatch**: latency, availability, retries, throughput, and state.

The semantic boundary creates a **correctness mismatch**: uncertainty about intent, judgment, authority, and accountability.

Speed makes coordination difficult. Semantic uncertainty makes trust difficult.

## Three interaction patterns

Native speed, kind of processing, lifecycle phase, and call direction combine to explain why the three interaction patterns—an agent writes code, calls code, or is called by code—are not equivalent.

### 1. An agent writes code at design time

Agent ──writes──▶ Code

The agent produces a software artifact. The work already occurs on a development timeline, where waiting for inference is often tolerable.

The artifact also creates a boundary before runtime. It can be inspected, compiled, tested, reviewed, revised, and approved before it becomes part of the executing system.

Those checks do not prove that the software has the right architecture or satisfies every unstated expectation. But software engineering has unusually strong mechanisms for checking many properties of code.

The agent participates in design time; the established code–CPU relationship still governs runtime.

### 2. An agent calls code at runtime

Agent ──calls──▶ Code (as tool)

Here, the slower participant owns the control loop and calls a faster deterministic capability.

"Tool" describes the role that code plays in this pattern: code is exposed as a callable capability. It is not a separate kind of participant.

The agent may decide that a refund is appropriate and call refund(order_id, amount).

The refund code does not need to understand the agent's complete reasoning. It can validate the account, order, amount, permissions, and transaction rules, then execute or reject a precise operation.

The agent can still make the wrong semantic decision. Calling code does not solve uncertainty. But deterministic software is not being asked to interpret the agent's reasoning; it is being asked to enforce a bounded operation.

### 3. Code calls an agent at runtime

Code ──calls──▶ Agent; Agent ──returns semantic judgment──▶ Code

This reverses the direction.

The faster deterministic application waits for slower, variably timed inference. When the answer returns, the application must use a semantic judgment to decide what happens next.

A program calls a library without concerning itself with whether the library is right. That was never the program's concern. It was ours, and we settled it before shipping: we read the specification, we ran the tests, we knew the behavior.

That is what trust has always been: not a runtime check, but a position we could reach and finish.

We cannot reach that position with an agent.

We can constrain the shape of its answer. Code cannot read a paragraph of English and work out what was decided, so asking for JSON moves that translation to the participant capable of it.

But shape is not behavior. We cannot predict an agent's reasoning, or settle in advance what it will conclude about an input we have not seen.

This relationship crosses both boundaries in their most direct form.

## Six questions

Saying that an application "uses an agent" tells us very little.

We need to ask:

- Who calls whom?
- Who owns the control loop?
- Who waits?
- Who consumes the semantic judgment?
- What manages the operational boundary?
- What limits the consequences of a wrong judgment?

Agents have not made the stable relationship obsolete. They have expanded it with new participants, directions, and control loops.

The interaction patterns are all technically possible. But they are not necessarily equally natural, equally scalable, or equally useful.

That leaves a more practical question:

Where does traditional software genuinely need to call an agent, wait for its semantic judgment, and use that judgment to perform its own business function?
