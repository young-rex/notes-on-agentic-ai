# We Built the World Around the CPU. We Are Doing It Again Around the LLM.

*Originally published on LinkedIn: <https://lnkd.in/p/gZ4QkT-Z>*

Three years ago Beren Millidge put the LLM in the processor slot: the model is the processor, the context window is RAM [1]. I am not borrowing that as a figure of speech. The fit holds at the processor, at the stack that grows around it, and — most of all — at the arc that growth follows over decades.

We have built the world around a processor once already — the software layers themselves, the ways of thinking that came with them, and the professions and ways of working that grew up around both. All of it took decades. We are doing it again, from the start, for a processor that handles a different kind of data. But not on empty ground.

That first build gave us the CPU stack, and enterprise software on top of it. The LLM stack is giving us a new class of application altogether — one that produces the analysis, writes the report, works the case. That was a person's job, not a program's. And the dependency is no longer one-way: the LLM stack also writes software for the CPU stack beneath it. Partially, unevenly, but it writes the software the older stack is made of.

## Two processors, two kinds of data

Both processors operate on encoded representations. Only one had the meanings of its instructions assigned by a designer.

A CPU's instruction semantics are stipulated in an architecture specification. `ADD R1, R2` has defined behavior, and that behavior is written down. An LLM's response to "Summarize this" is learned rather than specified. No human-readable contract fully defines it, and no compatibility guarantee carries it forward when the model changes.

**Stipulated versus learned.** That is the difference that matters here, and it has a consequence you can feel in practice: your prompt might break on the next model. Your assembly does not break on a newer chip implementing the same instruction set.

This is the processor-level version of the semantic boundary from the first post. `expected == actual` works when expected behavior can be specified; learned behavior gives us no equivalent general contract for whether an answer satisfied natural-language intent.

This also answers the objection that an LLM is "just matrix multiplication over token IDs." At the mechanical level, correct. It is also true of the CPU that it is just voltages over transistors. The difference was never the mechanism. It is where the meaning came from.

Now the part that matters for everything after this.

**A processor alone is not a system.** It provides no durable application state, external action, or orchestration. Those come from the stack around it.

Everything else is stack. So the need for one is not about the LLM being new, or unreliable, or badly behaved. It is structural. Any processor needs it.

## What surrounds a processor

Here is what that looks like today, in working code.

```ts
// Pi · packages/agent/src/agent-loop.ts · runLoop()
// Trimmed for reading: event emission, live steering, and the outer loop
// that exists to serve it. All of it harness.

async function runLoop(
  initialContext: AgentContext,
  newMessages: AgentMessage[],
  initialConfig: AgentLoopConfig,
  signal: AbortSignal | undefined,
  emit: AgentEventSink,
  streamFunction: StreamFn,
): Promise<void> {
  let currentContext = initialContext;
  let config = initialConfig;
  let hasMoreToolCalls = true;

  while (hasMoreToolCalls) {

    const message = await streamAssistantResponse(currentContext, config, signal, emit, streamFunction);

    newMessages.push(message);

    if (message.stopReason === "error" || message.stopReason === "aborted") {
      return;
    }

    // Check for tool calls
    const toolCalls = message.content.filter((c) => c.type === "toolCall");

    const toolResults: ToolResultMessage[] = [];
    hasMoreToolCalls = false;
    if (toolCalls.length > 0) {
      // A "length" stop means the output was cut off by the token limit, so
      // every tool call in the message may carry truncated arguments. Fail
      // them all instead of executing potentially borked calls.
      const executedToolBatch =
        message.stopReason === "length"
          ? await failToolCallsFromTruncatedMessage(toolCalls, emit)
          : await executeToolCalls(currentContext, message, config, signal, emit);
      toolResults.push(...executedToolBatch.messages);
      hasMoreToolCalls = !executedToolBatch.terminate;

      for (const result of toolResults) {
        currentContext.messages.push(result);
        newMessages.push(result);
      }
    }

    // … 20 lines elided: per-turn reconfiguration (model, thinking level, context) …

    if (
      await config.shouldStopAfterTurn?.({
        message,
        toolResults,
        context: currentContext,
        newMessages,
      })
    ) {
      return;
    }
  }
}
```

That is `runLoop()` from Pi, a small open-source agent harness [2]. I chose Pi because its entire agent loop is short enough to read in one sitting.

Find the line that calls the model. There is one.

> Everything else in that function is state, external action, and control over repeated calls.

And what you just read is the trimmed version. I took out lines to make its main logic stand out. All of them were harness too; the full function is one hundred and twenty-one lines.

The real thing is more lopsided than what you just read, not less.

I am not going to walk through the logic. The shape is the point — and every agent stack contains some version of this loop, whether the team wrote it or inherited it from a framework.

That is what an early stack looks like from inside it.

## The ladder we climbed last time

The software layers are the visible part. The rest is conceptual and organizational: the ways of thinking we developed in order to work at all, and the professions, roles, and methods that formed around them. All three took decades. All three are now invisible.

These rungs name functions the stack must fill, not a promise that the same mechanisms will fill them. A learned target may require different materials — evals, behavioral specifications, compatibility contracts, and adapters where stipulated semantics once made deterministic translation possible.

| The rung we climbed | What it solved | Where the LLM is |
|---|---|---|
| High-level languages | Code outlived the chip it ran on | Prompts are machine-specific; every upgrade needs revalidation |
| Object orientation, and paradigms generally | An agreed way to structure large programs | No settled way to structure an agent program |
| The split between systems and application programming | Different concerns, different skills, different people | The boundary between harness and application remains unsettled; one team often handles both |
| Information systems as a discipline apart from computer science | A different question, a different curriculum, often a different faculty | The roles and disciplinary boundaries of "AI engineering" are still forming |
| Waterfall, then agile | How to plan work under uncertainty | No settled methodology for planning agent work |
| Sequential, then concurrent, then distributed | Software kept scaling as hardware multiplied instead of speeding up | One loop; subagents are roughly concurrent; no settled distributed semantics |

Read that right-hand column on its own. It is almost entirely absences.

The first rung is worth dwelling on, because it is the one everybody has felt without naming. Prompts carry no guarantee across a model upgrade. Many of them survive it; nothing promises they will, and finding out means testing. That is why model vendors publish migration guides telling developers to re-tune their prompts, re-baseline their token counts, and remove instructions that worked on the previous version [3]. A Python application is normally insulated from a routine CPU replacement. Prompts have no comparable portability guarantee across model changes.

We are still writing prompts the way we once wrote assembly.

Six rungs. The LLM is on the first one, and only barely.

That is the diagnosis, and it is not the whole of it. The part nobody has built at all is not the processor, and not the stack. It is the relationship between them and us. What arrived is not a new tool in an old arrangement. It is a new party in it — one that acts on us and on the machine, and is acted on by both.

What an enterprise does about that — while the stack stays unfinished — is the next post.

---

## References

[1] Beren Millidge, *Scaffolded LLMs as natural language computers*, 11 April 2023. <https://www.beren.io/2023-04-11-Scaffolded-LLMs-natural-language-computers/>

[2] Pi, `packages/agent/src/agent-loop.ts`, `runLoop()`. earendil-works/pi (Mario Zechner), MIT License. <https://github.com/earendil-works/pi/blob/24bace27cf308c89707cf8005b4795d873e23f17/packages/agent/src/agent-loop.ts#L155-L275>

[3] Anthropic, *Claude model migration guide*. Instructs developers to "re-tune length and verbosity prompts", to "re-baseline cost and latency on your own workloads" and "re-test any client-side token-count estimations", and to "remove explicit verification or self-check instructions carried over from prompts tuned for earlier models". <https://platform.claude.com/docs/en/about-claude/models/migration-guide>
