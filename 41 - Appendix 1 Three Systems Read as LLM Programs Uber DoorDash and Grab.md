# 41-A1. Three Systems Read as LLM Programs: Uber, DoorDash, and Grab

*Originally published on LinkedIn: <https://lnkd.in/p/gvP7rCBd>*

**Appendix 1 to:** Post 41. [From LLM Calls to LLM Programs](https://lnkd.in/p/gNyPnB4U)

Post 41 defined an LLM program: application code composes LLM calls and defines the procedure, and a model's output can select a path only where the code declares one. It used a hypothetical program for shortening a customer notice as an example.

This appendix presents three systems described in engineering articles by the companies that built them: Uber's QueryGPT, which turns employees' questions into SQL; DoorDash's Dasher support automation; and Grab's data classification. On my reading, each described procedure is consistent with an LLM program. Each also shows a different arrangement under code: at Uber, stages it calls agents prepare context for the call that writes the query; at DoorDash, an LLM review runs only when a cheaper check flags an answer; at Grab, code batches the tagging requests, and a second version split the task in two.

For each system, I describe the task, how the procedure works, and the reported results, and I ask Post 41's two questions: who defines the procedure, and who selects the path during a run. The first question is the test. Post 41's definition lets a model's output select a path but does not require one, so an LLM program can be a fixed sequence. Where an article leaves something out, I mark it unreported rather than fill it in. The descriptions and figures below refer to the systems as reported in the cited 2023–2024 articles.

## System 1: How does QueryGPT turn a question into SQL?

Uber's data platform handles about 1.2 million interactive queries a month, and its Operations organization contributes about 36% of them [1]. QueryGPT takes an employee's question in plain language and returns a SQL query, with an explanation of how it was generated. The objective is to save authoring time.

The first version, built at a May 2023 hackathon, used one retrieval step and one generation call. It matched the question against a small sample set, fetched "3 relevant tables and 7 relevant SQL samples," and sent them to the model with custom instructions. As more tables were added, Uber reports, accuracy fell, and similarity search on plain-language questions returned irrelevant results. Some tables span over 200 columns and could use as much as 40,000 to 60,000 tokens in a request, while the largest model then available "only supported 32K tokens."

The design Uber described in 2024 keeps query generation and adds a layer of configuration and three stages. Workspaces and intent narrow retrieval, column pruning addresses the token limit, and table selection answers later feedback from users. Uber presents them in this order:

**Workspaces.** Curated configuration groups tables and SQL samples by business domain; users can also create their own workspaces. This gives the model a narrower field.

**Intent.** An LLM call maps the question to one or more workspaces and, by extension, to the tables and SQL samples mapped to them. Uber explains: "Picking a business domain allowed us to drastically narrow the search radius for RAG."

**Table selection.** Uber describes "another LLM agent," which proposes tables; the user selects "Looks Good" or edits the list. This addresses feedback from users who saw that the tables QueryGPT picked "were not correct."

**Column pruning.** An LLM call removes irrelevant columns to address "token limit issues," even with a 128K-token model.

**Query generation.** An LLM call generates the query. This was already in the first version.

The article explicitly puts intent first: "Every incoming prompt from the user now first runs through an 'intent' agent." It also places table confirmation and column pruning before query generation, without specifying the complete control flow.

Uber calls three of these stages agents: intent, table selection, and column pruning. By Post 41's test, the label does not settle the arrangement; what matters is who defines the procedure, and the article does not describe any stage generating or ordering the task's steps. Uber describes its intermediate agents as decomposing the user's prompt "into better signals for our RAG." Their outputs choose which workspaces to search, which tables to propose, and which columns to keep, so they select context for later calls. The user confirms or edits the tables. Even if one of those stages were itself an agent, the whole could still be a program at its own level: Post 41 allows a program to hand a step to an agent call.

A procedure that exists before the run can be "read, reviewed, and tested like other code," as Post 41 put it, and Uber's evaluation measures parts of its procedure separately. The evaluation set pairs real questions with the correct intent, tables, and SQL. One flow measures baseline performance from the question alone; another supplies the correct intent and tables, measuring performance "with the human-in-the-loop experience." Because that flow starts from correct upstream inputs, it measures the later stages on their own. The article reports usage and time savings rather than overall accuracy. A limited release to some Operations and Support teams averaged about 300 daily active users, with "about 78% saying that the generated queries have reduced the amount of time" they would have spent writing them. Uber estimates about 10 minutes to author a query by hand and about 3 with QueryGPT. It also says hallucinated tables and columns remain "an area that we haven't completely solved yet." It is "looking to include a 'Validation' agent that recursively tries to fix hallucinations," but does not say whether code or the agent would define the retry procedure.

**Uber calls three of its stages agents, but none is described as defining the procedure: they prepare context for the call that writes the query, and a person confirms the tables. Because the stages are fixed, Uber can also measure them separately.**

## System 2: How does DoorDash check an answer before a Dasher sees it?

DoorDash built a chatbot that answers support questions from Dashers, its delivery couriers [2]. It takes a Dasher's conversation and returns an answer drawn from DoorDash's knowledge-base articles; the objective is an answer that is accurate and compliant. The article also says the LLM can perform actions by calling APIs. It does not describe that part, so this reading covers only the procedure that writes and checks an answer.

The live procedure starts with an LLM call that summarizes the conversation; the article's Figure 2 labels it "LLM Summarizer." The summary is used to retrieve similar past cases and their knowledge-base articles, which go into the prompt for the call that writes the answer. A guardrail then checks the answer in two tiers. An inexpensive semantic-similarity check runs first: "If a response closely matches an article, it's less likely to be a hallucination." A close match goes to the Dasher. Only when the check flags a response does an LLM evaluator run, examining groundedness, coherence, and compliance. This limits use of the costlier evaluator. In Figure 1, an answer that fails the guardrail goes to a human agent, and Figure 3 labels that outcome "Trigger Fallback." The text adds that the process may retry with a new guardrail check, without saying when it retries, how often, or how retries relate to the fallback.

As described, code sets the steps and their order: summarize, retrieve, write, check, then send or fall back. An LLM's judgment selects the path at one declared point: the evaluator's verdict on a flagged answer. Post 41's notice program also runs its cheaper check first, and in both, code decides whether a later judgment can clear an earlier flag. The notice program's check tests its requirements directly, so no review can clear a failure. DoorDash's similarity score only approximates grounding, so a flagged answer goes to the evaluator for further assessment.

A separate process monitors quality. DoorDash's team reviewed thousands of chat transcripts to define five quality aspects, and built monitors for them from LLM prompts or regular-expression rules. An "LLM Judge" answers open-ended questions about past transcripts; the answers are summarized into common issues, and the frequent ones "are then built into prompts or rules for further monitoring." Separately, the team improves the knowledge base, retrieval, and the chatbot's prompts, and "a suite of predefined tests is triggered by any prompt changes, blocking any failing prompts." This is people changing the system between runs, with tests gating prompt changes. It is not part of answering a Dasher, and it is not evidence that a model defines the live procedure at runtime.

DoorDash reports that the guardrail "reduced overall hallucinations by 90% and cut down potentially severe compliance issues by 99%," and that the system "autonomously assists thousands of Dashers" each day. It does not disclose the baseline counts, sample size, evaluation period, or calculation method behind those percentages.

**In DoorDash's described procedure, an LLM's judgment selects the path at one declared point: when a cheap check flags an answer, an LLM evaluator decides whether it goes out or falls back to a human agent. The reported gains come without the details needed to weigh them.**

## System 3: How does Grab tag data at scale?

Grab needed to classify sensitive data across petabytes of database tables, message schemas, and data lake tables [3]. Manual tagging had not kept up. Data producers tagged whole schemas, so "half of all schemas were marked as Tier 1, enforcing the strictest access control measures," and the data lake lacked table-level classification. The system takes the schema of a data entity and publishes a tag for each column to downstream platforms; the objective is the most appropriate tag for each column.

The procedure is a batch pipeline. Data platforms send scan requests to an orchestration service, which groups them into mini-batches at fixed intervals and rate-limits calls to the model provider. An LLM call acts as a "column tag generator," assigning one tag per column, with a default tag of **&lt;None&gt;** "when it cannot make a decision or is confused." The predictions are published downstream, and the platforms ask data owners each week to verify them. As described, code sets the batch steps, and the LLM call and the owners act at points the pipeline declares.

The second version changed the decomposition [4]. Expanded evaluation and testing data had uncovered "weaknesses in the previous model," and Grab's team "hypothesised the model's main issue was model capacity": 13 of its 21 tags distinguished kinds of non-PII data. The team split the work into a PII task and a task for everything else, on the principle that "large tasks are divided into smaller, more manageable tasks." The PII task now chooses among 8 tags instead of 21, because the non-PII tags moved to the other task. The team also shortened the prompt and split tables with more than 150 columns. Owners still verify, and their feedback, with the data governance office's own classifications, became training and test data.

With two tagging tasks and wide tables split, version 2 suggests more than one call per entity, though the exact arrangement is unreported; the article does not say whether the two tasks run in sequence or in parallel. Each scan request can then be read as a program call, in Post 41's terms: a data platform submits a data entity to the tagging service as one operation and receives the predicted tags on a queue. Owner verification follows that call; it is not part of it.

The results are reported loosely. In its first month, the first version scanned more than 20,000 data entities [3]. For acknowledged tables, owners changed "less than one tag" on average, and 80% of owners surveyed said the process helped. Version 2 reports "exceptionally low misclassification rates" without figures. Grab's earlier article [3] also named next steps: removing the verification mandate once accuracy reaches a certain level, and experimenting with confidence output so that Grab would "only trouble users when the LLM is uncertain of its answers."

**At Grab, code runs the batch, an LLM call tags each column, and owners verify the tags afterward. Version 2 split the tagging into two tasks, which makes each scan request readable as a program call.**

## What the three systems show

In all three accounts, as I read them, code defines the procedure. What would count against that reading is an account of a model generating or ordering the steps at the level examined, and none of the articles gives one. On the second question, only DoorDash reports a branch on an LLM output. Grab splits tables with more than 150 columns, but that condition is on table size, not on an LLM output. At declared points, people confirm proposed tables, take over answers that fail DoorDash's guardrail, or verify predicted tags.

The table groups LLM outputs by role. Post 10 placed the LLM stack's judgment in "context selection, loop termination, and graph routing." Context selection and routing appear here; no account reports loop termination. Content is the work product itself.

| System | Branch on an LLM output | LLM outputs, by role | People |
|---|---|---|---|
| QueryGPT | none reported | context selection: workspaces, proposed tables, pruned columns; content: the query | confirm or edit the proposed tables |
| DoorDash | the evaluator decides whether a flagged answer goes out or falls back to a human agent | context selection: the conversation summary; content: the answer; routing: the evaluator's verdict | take over answers that fail the guardrail |
| Grab | none reported; tables with more than 150 columns are split | content: column tags | verify the tags weekly |

Post 32 sorts software by what carries the business work: in C2, the LLM does it and code only supports it; in C3, code and the LLM each carry part of it. On my reading of each described procedure, the code only supports the model, so all three are C2 by Post 32's test. QueryGPT's business knowledge enters as workspaces, samples, and instructions; the article describes no business rule in code. DoorDash's similarity tier screens answers for model error, while the LLM evaluator judges groundedness, coherence, and compliance; an answer that passes the similarity tier goes out without that review, and no described code tests answers against company policy. DoorDash's API actions, which its article does not describe, fall outside this classification. Grab's first article presents tiering as a downstream use: the tags, "in combination with rules provided by data privacy office," determine each entity's sensitivity tier [3]. Grab's procedure would become C3 if those rules run in code within it; neither article [3, 4] says.

Each article also leaves a design choice open. Uber plans a "Validation" agent but does not say whether code or the agent would define its retries. DoorDash mentions retries without limits or conditions. Grab planned to experiment with confidence output, so owners would verify only when the model is uncertain. If code defined them, retries and confidence-gated verification would add branches on LLM outputs, and the procedures would remain LLM programs. Even an agent inside Uber's validation stage would not necessarily change how the outer procedure reads.

The articles also differ in how a result is accepted. Uber reports no acceptance check at runtime; its "Successful Run" and "Run Has Output" signals belong to evaluation. DoorDash accepts an answer through its guardrail and falls back to a human agent, but leaves the retry details incomplete. Grab publishes its predictions before owners verify them.

All three systems also split work across LLM calls: Uber into stages; DoorDash by "breaking down complex prompts into smaller, manageable parts and employing parallel processing where feasible"; and Grab, in its second version, into two tagging tasks. None reports a figure that isolates what splitting the work contributed. A later post asks when splitting work across LLM calls earns its cost.

---

## References

[1] Jeffrey Johnson, Callie Busch, Abhi Khune, and Pradeep Chakka, *QueryGPT – Natural Language to SQL Using Generative AI*, Uber Engineering, September 19, 2024. <https://www.uber.com/us/en/blog/query-gpt/>. The operator's own article. Time savings are user-reported or Uber's estimates, and no overall accuracy figure is reported.

[2] Zhe Jia, Shuai Wang, Aditi Bamba, and Martin Wang, *Path to high-quality LLM-based Dasher support automation*, DoorDash, September 17, 2024. <https://careersatdoordash.com/blog/large-language-modules-based-dasher-support-automation/>. The operator's own article. The baseline counts, sample size, evaluation period, and calculation method behind its reported reductions are not disclosed.

[3] Hualin Liu, Stefan Jaro, Harvey Li, Jerome Tong, Andrew Lam, Chamal Sapumohotti, Feng Cheng, and Aaqib Kufran, *LLM-powered data classification for data entities at scale*, Grab Engineering, October 2023, updated July 15, 2024. <https://engineering.grab.com/llm-powered-data-classification>. The operator's own article. It also gives a daily average and a yearly effort-saving estimate; I omit both because the article does not show how they relate to its monthly total.

[4] Nick Buhrer, Shreyas Parbat, and Yucheng Zeng, *Metasense V2: Enhancing, improving and productionisation of LLM powered data governance*, Grab Engineering, November 14, 2024. <https://engineering.grab.com/metasense-v2>. The operator's own article; no accuracy figures are given.

## Earlier in this series

All posts are archived in the GitHub repository: <https://github.com/young-rex/notes-on-agentic-ai>

- Post 41: *From LLM Calls to LLM Programs* <https://lnkd.in/p/gNyPnB4U>
- Post 32: *AI Coding Has Two Sides. Ours Starts with What Runs the Software.* <https://lnkd.in/p/gacN7SgW>
- Post 10: *The CPU Stack Runs on Specification. The LLM Stack Runs on Judgment.* <https://lnkd.in/p/ga3apNfd>
