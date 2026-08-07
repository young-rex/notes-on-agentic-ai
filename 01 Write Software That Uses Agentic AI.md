# Write Software That Uses Agentic AI

Generative AI got serious the moment it started writing code. Skip past the early stages — autocomplete, then "vibe coding" — because the interesting shift came after.

Everyone is on one path: *using AI to write software.* It's crowded and getting more so. But there's a second door far fewer people walk through — **writing software that uses agentic AI.** Not using the AI to produce your code, but building a system in which the AI is a running component. Those are different jobs, and the second is much harder than it looks.

The difficulty isn't only a matter of skill that time will fix. It comes from a difference between two kinds of processing.

We understand CPU-based processing extremely well. At the bottom, a CPU only ever operates on data — even its instructions are data. On top of that sits a foundation the field agrees on: a shared model of what the machine does, and a shared definition of what "correct" means. `Algorithms + Data Structures = Programs` [1]: input → a process driven by an algorithm → output. And when a precise specification exists, the output is mechanically checkable — a program compares what it got against what it expected and decides, deterministically, whether it matches. That agreed ground is what lets everything stack: the OS trusts the hardware, the libraries trust the OS, the frameworks trust the libraries.

AI-based processing has no such common ground yet — no agreed way to decide when an output is right. It takes natural-language intent as input, which is exactly why it feels so convenient. But even when its output is structured — a tool call, a JSON object, a code change — there is no cheap, deterministic check that the output *satisfies the intent* the way `expected == actual` does. So when you build software that uses agentic AI, you are asking a deterministic program to consume, judge, and act on an output it cannot trivially verify. That uncertainty doesn't stay at the bottom of the stack. It propagates upward.

That is the root problem, and it's worth saying plainly: we do not have a cheap, deterministic, general-purpose answer to it yet [2]. Most people meet it only as symptoms — a flaky agent, a demo that worked once — without seeing the single question underneath.

We cope by shrinking the problem: smaller scope, tighter context, structured output. Those help, but they constrain the *structure* of what the model returns, not the *choice* inside it — the hard, semantic half stays open. Naming that question at the root, instead of fighting its symptoms one at a time, is where writing software that uses agentic AI actually begins.

---

## References

[1] Niklaus Wirth, *Algorithms + Data Structures = Programs*, Prentice-Hall, 1976.

[2] Li et al., *From Generation to Judgment: Opportunities and Challenges of LLM-as-a-judge*, EMNLP 2025. (arXiv:2411.16594. Evaluating open-ended natural-language output remains an open challenge; traditional matching-based metrics such as BLEU and ROUGE "fall short in open-ended and dynamic scenarios.")
