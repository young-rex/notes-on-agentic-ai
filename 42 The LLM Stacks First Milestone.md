# 42. The LLM Stack’s First Milestone

*Originally published on LinkedIn: <https://lnkd.in/p/gFsPpppz>*

Post 00 set three stacks side by side, with the CPU, the LLM, and a person as their processors. For the LLM stack, it left a question open: “What drives the LLM stack, and what is its ‘program’?”

Post 34 named the LLM call, the stack’s one processor-level operation, and the agent call, its first stack-level operation. Post 41 added the program call as the second stack-level operation and named what it invokes, the LLM program. Between them, the two posts give Post 00’s question a first answer. In an agent call, the model drives the work, and the procedure takes shape as it runs. In a program call, code drives it, through an LLM program written before the run.

What do the three operations add up to? This post follows one task through all three, sets them side by side, and argues that together they mark the LLM stack’s first milestone. The series now has a first account of how the stack composes work. One processor-level operation supplies inference. Two stack-level operations organize it into tasks. Together they let us trace a whole task and see where model judgment and procedural control sit.

## 1. One notice through each call

Take the customer notice from Post 41. Shorten it to at most 120 words while keeping certain sentences word for word.

An LLM call is one step of inference. It gives the model one context and gets back one response. The response can be a draft or a request to call a tool, such as a word counter. As Post 34 put it, the LLM processor “runs no tools, and doesn’t decide what happens between calls.” Its caller, such as a harness, decides whether to run the tool, whether to call the model again, and what goes into the next context.

An agent call gives the model a task and lets it decide what to do next. Give it the notice, the limit, the required sentences, and a word counter. A harness runs the loop. It assembles each context, carries out the actions the model is permitted to request, feeds back the results, and stops at a final response or a limit. Inside that loop, the model chooses the steps and their order. It can count the words first or check the sentences first, revise after either check, and decide when the draft is done. In Post 41’s terms, the model defines the procedure as it works.

A program call hands the task to an LLM program, whose code defines the procedure before the run. Post 41’s notice program drafts, checks the word count and the required sentences in code, and only then asks a review call for a verdict. If a check fails or the review rejects the draft, the next attempt revises it with the reasons, up to three attempts in all. Model judgment still matters. The drafts come from LLM calls, and the verdict chooses between accepting and revising, but only at the point the code declares. Model output can pick a branch or end the loop. It cannot add a step the code does not contain.

**The same notice can go to each of the three calls. An LLM call gives one step, and its caller decides the rest. In an agent call, the model chooses the steps as it works. In a program call, code defines them before the run.**

## 2. Four hundred notices through all three calls

The three operations also nest. To see how, scale the job to four hundred notices and run it on Anthropic’s Managed Agents, which “provides the harness and infrastructure for running Claude as an autonomous agent” [1]. A developer configures an agent with a model, a system prompt, and tools. Each session runs an instance of that agent on a task [1]. With dynamic workflows, a beta feature, the session’s agent can write code that runs other agents and combines their results. Anthropic’s server executes it in the background as a workflow run [2]. In this series’ terms, that workflow is an LLM program.

The session’s agent writes a workflow for the job and starts the run in the same turn [2]. The workflow could assign each notice to a drafting agent, check the word count and required sentences in code, and make a separate agent call to review each draft that passes.

Anthropic’s documentation describes a loop like Post 41’s. The workflow program “can have a draft revised until a review passes or a set number of rounds is used up” [2]. Anthropic caps how many agents a workflow can start in one run [2]. To allow several attempts for every notice, the session’s agent could split the batch across runs.

The session’s agent can keep working while a run goes on, or end its turn and let the run continue. When a run ends, even with an error, the agent normally gets another turn to read the result without a new request from the caller [2].

Apply Post 41’s test at each level. Who defines the procedure? The outer level is the session’s agent call. The model writes the workflow while the task is underway, so the model defines the procedure there. The workflow run is a program call. Once it starts, its code defines the procedure. Each agent returns its result to the program, and the program determines which agents run next and writes their prompts [2]. An agent’s result can select the next step, but only among the steps the code contains. Each of those agents is an agent call again, deciding through its own LLM calls how to carry out its assignment.

In this example, the outer agent call starts when the caller submits the batch and ends when the agent reports the task’s outcome or the harness stops the call. While the drafting agent is making an LLM call for notice 17, the workflow is waiting for the draft. The outer agent call is still pending, even if its launch turn has ended with a progress update. Post 41 said the agent call and the program call “nest both ways.” An agent can make a program call, and a program can hand a step to an agent call. Here they do both in one task. Managed Agents is one way to build this, not the only one. Any agent with a tool that runs code could write an LLM program and start it.

**A single task can use all three operations together. An agent writes an LLM program and invokes it through a program call. The program makes agent calls, and every agent works through LLM calls.**

## 3. The three side by side

At the caller’s boundary, the three operations share one shape: one input in, one output out. A context yields a response, and a task yields a result. Inside each boundary, the work differs.

| | LLM call | Agent call | Program call |
|---|---|---|---|
| Level | Processor level | Stack level | Stack level |
| Input and outcome | A context in, a response out | A task in, a result out | A task in, a result out |
| Who defines the task procedure | Its caller; the call is one step | The model, as it works | Code, before the run |
| CPU-stack counterpart | An instruction | A function call | A function call |

Post 41 separated two questions: who defines the procedure, and who selects a path through it at runtime. The review verdict in the notice program shows the difference. Code defines the two branches, accept and revise, and the model’s verdict selects between them. In an agent call, the model does both as it works, inside the loop its harness runs.

**At the caller’s boundary, a context yields a response and a task yields a result. Inside, model judgment sits in every LLM call, and procedural control sits with whoever defines the procedure.**

## 4. Why this is a milestone

Post 7 put the LLM in the processor slot and set the LLM stack against the CPU stack. Carried down to operations, that comparison pairs the LLM call with the instruction. It pairs the function call with both stack-level operations the series has identified so far, the agent call and the program call.

The two relate to the function call differently. In a function call, code defines the procedure before the run, and data selects a path through it. Post 34 used C’s **strlen(s)** as the example: the code defines the loop, and the string decides how many times it runs. A program call keeps that shape, and as Post 41 put it, a model’s output plays the string’s part. An agent call departs from it, because the model shapes the procedure while it runs. That difference is why the series names two stack-level operations, not one.

Where does a person fit? Someone steering a chat chooses what to ask for next. Within a turn, the system can compose several calls to answer. In this account, that person is a caller from the human stack, the third stack in Post 00. The person directs the conversation, and the work inside each turn runs through the stack’s operations.

In the notice program, code sends the original and shortened notices to the model for review. It uses the verdict to decide whether to accept the draft. The CPU and LLM stacks interoperate technically through exchanges like this one. The three calls let us trace those exchanges across a whole task. Post 11 pointed to the remaining limit. Whether the shorter notice preserves the intended meaning still calls for judgment.

This account is a first milestone, not a finished foundation. Post 34 said of the stack level, “more can follow,” and more still can. Contracts around model judgment remain partial, as Post 10 argued. I drew the boundary around composition too tightly in Post 10. There I wrote that composition “works for the deterministic scaffolding around the model but not for the model’s contribution within it.” The notice program shows that model judgment can take part in the composition under a partial contract.

In the nested workflow, even the code comes from judgment. The agent writes it and starts the run in the same turn, with no built-in approval step [2]. The workflow can begin before a person reviews the code. The next milestone is an account of the semantic side of these calls and what it takes to trust the judgments they carry.

**One processor-level operation and the two stack-level operations identified so far let the series trace a whole task down to inference and locate procedural control at each level. Through them, the CPU and LLM stacks interoperate technically. That is the first milestone. The semantic side is next.**

---

## References

[1] Anthropic, *Claude Managed Agents overview*, Claude Platform documentation, accessed November 2026. <https://platform.claude.com/docs/en/managed-agents/overview>. Vendor documentation for a beta service.

[2] Anthropic, *Workflow runs*, Claude Platform documentation, accessed November 2026. <https://platform.claude.com/docs/en/managed-agents/workflow-runs>. Vendor documentation for a beta feature.

## Earlier in this series

All posts are archived in the GitHub repository: <https://github.com/young-rex/notes-on-agentic-ai>

- Post 41: *From LLM Calls to LLM Programs* <https://lnkd.in/p/gNyPnB4U>
- Post 34: *Defining the LLM Stack: One Operation and Messages* <https://lnkd.in/p/gBRCT5R6>
- Post 11: *The Models Are Getting Better. Code Still Cannot Fully Check Their Judgment.* <https://lnkd.in/p/gQqVvCsF>
- Post 10: *The CPU Stack Runs on Specification. The LLM Stack Runs on Judgment.* <https://lnkd.in/p/ga3apNfd>
- Post 7: *We Built the World Around the CPU. We Are Doing It Again Around the LLM.* <https://lnkd.in/p/gZ4QkT-Z>
- Post 00: *This series has formed a vision.* <https://lnkd.in/p/gZR-S3KV>
