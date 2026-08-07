# The Models Are Getting Better. Code Still Cannot Fully Check Their Judgment.

The last post reached a technical conclusion: the two stacks rest on different foundations — one on specification, one on judgment. Different foundations produce different stacks, and different stacks do not interoperate naturally. Code on the CPU stack cannot reach inside the LLM stack and check whether a judgment was right.

The enterprise question is direct — can vendors make that edge consumable? Can they make what crosses from the LLM stack to the CPU stack something code can verify?

For three years, they have been working on it. The work is real, and it has changed what is buildable. But the answer is: partially.

## Three kinds of mechanism

The mechanisms get discussed as one trend — models becoming more controllable. They are three different things. They work at different places, and only one of them changes what code can verify at the edge.

| Category | Mechanism | What it establishes | What it leaves open |
|---|---|---|---|
| **Constraint** | Function calling [1] — typed tool invocation with argument schemas | The interface: which operations exist and what shape their arguments take | Which tool to call, and what arguments to pass |
| **Constraint** | Structured output, enforced by constrained decoding [2][3] — a schema as contract, token masking as enforcement | The format: output conforms to the schema | The choice inside the structure |
| **Capability** | Reasoning models [4] — inference-time compute spent before answering | Better judgment on average | Whether any given judgment satisfies intent |
| **Authority** | Instruction hierarchy [5][6] — training the model to prioritize system instructions over user and tool input | A learned ordering of trust | Priority is learned behavior, not enforced isolation |

Constraints work at the edge. A schema governs what crosses from the LLM stack to the CPU stack — and code on the receiving side can verify it. The output is `{"route": "refund"}` — structured data code can consume, not only human language. That is real, and code can check it.

But the decision that produced `"refund"` happened inside the LLM stack, where the CPU stack cannot reach. Whether issuing a refund was the right decision is untouched. Verified shape, not verified judgment.

The other two categories do not change that edge. They improve what happens inside the LLM stack. A reasoning model makes better decisions on average, but a better-reasoned wrong answer is still wrong — it just arrives with more supporting text. Instruction hierarchy teaches the model to listen to the right voice, but teaching is different in kind from a permission system, which does not weigh anything. It denies.

Vendors can enforce what crosses the edge. They cannot tell code whether the judgment inside it was right.

---

Whatever code can check at that edge, it checks. What falls outside — the judgment the constraints did not cover — used to be a human's. The enterprise offloaded it to AI. When it is wrong, who answers for it?

That is the question the next post picks up.

---

## References

[1] OpenAI, *Function calling and other API updates*, 13 June 2023. <https://openai.com/index/function-calling-and-other-api-updates/>

[2] OpenAI, *Introducing Structured Outputs in the API*, 6 August 2024. On OpenAI's own complex-JSON-schema evaluation, `gpt-4o-2024-08-06` scored 100% with Structured Outputs, against under 40% for `gpt-4-0613`. <https://openai.com/index/introducing-structured-outputs-in-the-api/>

[3] Yixin Dong et al., *XGrammar: Flexible and Efficient Structured Generation Engine for Large Language Models*, arXiv:2411.15100, 22 November 2024. Integrated as a structured-generation backend by SGLang (November 2024), vLLM (December 2024), and TensorRT-LLM (January 2025), per the project's own integration record. <https://arxiv.org/abs/2411.15100>

[4] OpenAI, *Learning to reason with LLMs*, 12 September 2024 — introduces o1-preview. <https://openai.com/index/learning-to-reason-with-llms/>

[5] Eric Wallace, Kai Xiao, Reimar Leike, Lilian Weng, Johannes Heidecke, and Alex Beutel, *The Instruction Hierarchy: Training LLMs to Prioritize Privileged Instructions*, arXiv:2404.13208, 19 April 2024. <https://arxiv.org/abs/2404.13208>

[6] OpenAI, *Improving instruction hierarchy in frontier LLMs* (IH-Challenge training dataset), 10 March 2026. Trust order: System > Developer > User > Tool. <https://openai.com/index/instruction-hierarchy-challenge/>

---

## Earlier in this series

- [The CPU Stack Runs on Specification. The LLM Stack Runs on Judgment.](Post 10)
- [When a Stack Is Young, Old Problems Get New Names](Post 9)
- [Does Software Ever Need to Call an Agent?](Post 3)
- [Write Software That Uses Agentic AI](Post 1)