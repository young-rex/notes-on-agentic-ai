# 34. Defining the LLM Stack: One Operation and Messages

*Originally published on LinkedIn: <https://lnkd.in/p/gBRCT5R6>*

## The CPU stack is defined. The LLM stack isn't yet.

An LLM is the processor of a new stack, the LLM stack. Post 10 compared that stack with the CPU stack: the CPU stack runs on specification, and the LLM stack runs on judgment.

The CPU stack is well defined. "An instruction set defines exactly what each operation does," as Post 10 put it, and everything above is built from those operations. The LLM stack isn't defined yet. Post 10 said it will develop "its own mechanisms, its own contracts, its own forms of verification," and left them open: "What those mechanisms look like is an open question."

This post proposes first definitions, in two parts. Part 1 says what does the work in the LLM stack, and the operations it performs. Part 2 says how callers interact with them. Each part sets the LLM stack next to the CPU stack, because the contrast is what makes the definitions clear.

These are new ideas, not research, with just enough reasoning to follow them. The technical facts are ones most IT professionals will recognize. Research-style writing that tests the ideas will come later.

## Part 1: The LLM processor and its first two operations

### What counts as the processor

Start with the smallest operation a caller can ask for, and call it the stack's processor-level operation. Whatever performs it is the processor.

On the CPU stack, that operation is an instruction, and the CPU performs it. But a CPU can't serve its callers on its own. It needs a computer around it, and usually an OS.

On the LLM stack, that operation is the LLM call. A model file on disk serves nothing. A call is served by the model, the hardware it runs on, and the serving software that runs inference, which does for the model what the OS does for the CPU.

So I count all three as one thing: the LLM processor. The two boundaries differ because the operations do: the CPU alone performs an instruction, but an LLM call takes all three. That makes the LLM processor self-contained: it serves its callers on its own, which a CPU can't. Self-contained doesn't mean complete. It keeps no task state for its callers, since each call gets that from its context. It runs no tools, and doesn't decide what happens between calls. That comes from the stack around it.

### Processor-level operation: the LLM call

An LLM call reads one context and generates one response: a final output or a request to call tools. Tokens are generated inside it, but the call is the smallest unit a developer can direct.

It's the LLM's only processor-level operation. That sounds odd next to a CPU. RISC-V's base integer set has about 40 instructions, and x86-64 has nearly a thousand. Each does one exactly specified thing, so programs get their range by combining many of them.

An LLM is told what to do in language, inside the input, so its variety comes from what goes in and what comes out, not from choosing among operations. Settings such as an output schema or a token limit only shape the call, and a tool call is one kind of response, not a second operation.

On the CPU, range comes from combining many operations, because it runs on specification. On the LLM, it comes from one, because it runs on judgment. The LLM call's contract is partial, in Post 10's sense: it covers the format of the response, not the judgment inside it.

### Stack-level operation: the agent call

The second operation is one call to an agent on a task. The caller sees one operation. Underneath, a harness loops through LLM calls and tool calls until the model gives a final response or the harness stops it.

On the CPU stack, the closest match is a function call, which a programming language provides, not the CPU. Take one with a loop inside, like C's **strlen(s)**, where the data decides how many times the loop runs. The difference: in code, every next step follows rules written before the program runs. In an agent call, the model can pick the next step while it runs.

That's where the judgment sits, in what Post 10 called "context selection, loop termination, and graph routing." Agents differ in how much of that they leave to the model and how much they write into code. Whether an agent is packaged or built for the business, its call "hides the plumbing but leaves the judgment exposed," as Post 10 put it: the caller still has to check the result.

Agent calls nest like function calls: an agent can hand a subtask to a subagent.

Why "first two"? The processor level has exactly one operation. The stack level has its first, and more can follow.

**The LLM processor performs one operation, the LLM call. The agent call is the first operation built on top of it.**

## Part 2: How callers interact with them

### CPU: shared memory

In a computer, memory holds both the program and its data. The CPU's instructions work on its registers and that memory. Its callers are programs stored in that same memory and run by the same CPU. So callers interact with the CPU through shared memory: they read and write the same state, and moving data between memory and registers takes instructions of its own.

This keeps the CPU close to its memory. Reaching main memory takes about 100 nanoseconds. A round trip across a data center takes about half a millisecond, which is over a million CPU cycles. A CPU can't wait that long for each piece of data.

### LLM: message passing

The LLM processor's callers, such as harnesses, are programs running outside it, on CPUs. An LLM call takes one context from them and returns one response. Nothing else passes between them. That holds even when a model runs inside the caller's app on a laptop: the call is still one context in, one response out.

So callers interact with the LLM processor by passing messages. The caller's side of it is composing the message: deciding what goes into each context. That's the counterpart of moving data into a CPU's registers, and it's the harness's job, often called context engineering. The state of the work stays on the caller's side: in the harness, and in stores that tools reach.

Distance matters much less here. An LLM call takes seconds, so a half-millisecond round trip is barely noticeable. The LLM processor can sit anywhere.

An agent call works the same way one level up. A task goes in as a message, and a result comes back. A subagent gets its task as a message and works in its own context, which may include a copy of the parent's.

The tools already look like this. LLM APIs take and return messages, and Anthropic's is called the Messages API. Protocols follow the same pattern: MCP, which connects agents to tools, uses JSON-RPC messages, and A2A, which connects independent agents, exchanges messages and tasks.

The term names the contrast with the CPU's shared memory. As the LLM stack grows, its messages may become something native to LLMs, and the idea may get a different name.

### Side facts

A few things follow. None of them is the main point.

- **The LLM processor shares nothing with its callers.** Agents are different. A coding agent edits the same repo its caller uses. But it reaches the repo through tool calls, and the agent call itself is still a message.
- **The LLM's platform is no single machine.** The model keeps no state between calls, and its weights don't change while it serves them. So any copy of a model can serve any call, and an agent call can send its LLM calls to different models. Anthropic's research system runs its lead agent on Opus and its subagents on Sonnet. The CPU's platform is the von Neumann computer.
- **Some features look like shared state but aren't.** Prompt caching keeps state on the servers between related calls, but for speed and cost, not as task memory: each call still reads its whole context. Providers that store conversations rebuild them into the context for each call.

**Callers reach the CPU through shared memory. They reach the LLM processor and agents by passing messages.**

## The two stacks side by side

| | CPU stack | LLM stack |
|---|---|---|
| Processor | The CPU, which needs a computer and usually an OS | The LLM processor: the model, its hardware, and its serving software |
| Processor-level operations | Many instructions | One: the LLM call |
| A stack-level operation | A function call | The agent call |
| How callers interact | Shared memory | Message passing |
| Platform (a side fact) | The von Neumann computer | No single machine |

My reading: the LLM stack is growing around a processor that serves calls on its own and talks only in messages. That's a different starting point from the CPU stack's, and the layers built on it are likely to differ too.

These definitions are a start, and later posts will build on them.

**The LLM stack starts from one operation and messages, not many operations and shared memory.**

---

## Earlier in this series

All posts are archived in the GitHub repository: <https://github.com/young-rex/notes-on-agentic-ai>

- Post 10: *The CPU Stack Runs on Specification. The LLM Stack Runs on Judgment.* <https://lnkd.in/p/ga3apNfd>
