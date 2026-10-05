# 32. AI Coding Has Two Sides. Ours Starts with What Runs the Software.

*Originally published on LinkedIn: <https://lnkd.in/p/gacN7SgW>*

## Too many ways to cut it

Post 30 covered their side of AI coding: what model labs and agent builders built into the models and agents we use. Post 31 turned to our side and found it too big for one post. It published the findings as they were, broad and unranked, and suggested cutting them from many angles, such as industry, role, team, or way of working. It also said I might take my own cut in Post 32.

This is that cut, and it is not one the findings themselves make.

Every angle Post 31 suggested asks how software gets built: who builds it, where, and how. That is the question the findings were gathered to answer. Their scope is "using AI to develop software."

Every piece of software also answers a second question: what runs it once it is built. The findings leave that question open. Their scope line notes that the resulting software "may be conventional or may contain AI," but the findings do not separate the two.

The two questions are independent. Whatever the way of working, from pairing with an agent to describing an app and getting one back, AI can help build software that runs on code, on a model, or on both. So this post adds the run question to Post 31's build questions.

It takes the run question first, because the answer decides what building requires. Software that runs on code needs people who own the code. Software that runs on a model needs evaluation. Software that runs on both needs a boundary designed between them.

This post is a setup.

- **The first half** lays the foundations, drawn from earlier posts in this series: two stacks and two fundamental facts about them.
- **The second half** derives three categories of software from those foundations, and what each asks of the people who build it.

Real examples, with analysis, will come in Post 33.

## Two stacks

Everything below rests on one claim, so it comes first: **software can run on two stacks, and the two are separate.**

For decades, one relationship held underneath software. Post 2 put it in two lines: "Humans expressed intent through code. CPUs executed that code." The layers that grew around that arrangement, from instruction sets to operating systems, databases, and frameworks, form what Post 0 called the CPU stack, with the CPU as its processor.

Post 0 then named a second: "An LLM is the processor of a new stack." Call it the LLM stack.

The obvious objection is that an LLM runs on CPUs and GPUs. At the level of the device, it is one more program. Why is it not one more layer of the CPU stack?

Because every layer of the CPU stack is specified in terms of the layer below. In Post 10's words, "At every layer, someone could write a complete, precise contract for the behavior — and once written, it held." An instruction set, an operating system, and a database each define exactly what their operations do. From that one property, Post 10 traced a chain: contracts, then automation, abstraction, standardization, composition, and verification. The chain is why the CPU stack's guarantees compose, and why the stack could grow as tall as it did.

An LLM breaks the chain. The arithmetic underneath is fully specified; the judgment it produces is not. It was learned from data, not written down, and no contract fully defines what the model will conclude about an input it has not seen.

Post 10's title states the difference in one line: the CPU stack runs on specification; the LLM stack runs on judgment.

Like a CPU, an LLM is general. It takes instructions, written in language instead of code, and carries out a broad range of tasks. That makes it a processor, not a component.

So an LLM is not a layer of the CPU stack. It is the processor of a new stack, hosted on the old one. The same holds for people: they run on biochemistry, yet nobody treats management as a branch of chemistry.

Generality also decides where classical machine learning sits. A fraud-scoring model is learned too, but it is a fixed function, measured by an error rate, and it takes no instructions. It stays a component of the CPU stack. In this post, AI inside software means an LLM.

Post 0 also named a third stack, the oldest: people. What you build as software runs on the first two, so this post needs only those. But people never leave the picture. They come back below, as the ones who answer for what software does.

**An LLM is hosted on the CPU stack, not a layer of it.**

## Two fundamental facts

Two facts about the stacks do the rest of the work. Each guarantees something different.

**The first fact is about relative speed.** An LLM is itself a program running on the same kind of hardware as everything else. So for any job that ordinary code can do, ordinary code is faster than an LLM, in practice by many orders of magnitude. A business rule in code runs in thousands of instructions or fewer. A language model spends roughly two arithmetic operations for each parameter it uses, on every token it reads or writes. DeepSeek-V3 activates 37 billion parameters per token, so it spends about 74 billion operations on each token.

Smaller models and specialized hardware narrow the gap, but they cannot close it, because a model still needs billions of operations where code needs thousands. What does improve is the LLM's absolute speed, so more paths through a system can wait for it. But some paths run at the software's own speed. Post 3 called them the fast lane: request paths, transactions, and control loops. However fast models get, code remains far faster, so those paths stay with code.

**The first fact guarantees that pure CPU software will always exist.**

**The second fact is about specification versus judgment.** Code runs on specification; an LLM runs on judgment. A judgment that could be fully specified would be code. Code that needs judgment has to ask a model or a person. Work can move from judgment to specification as people learn to specify it. The line between them does not go away.

This fact supports the first guarantee on its own. A quarterly tax calculation has no latency pressure, and an LLM call would cost pennies. It still stays in code, because the result must be exact and auditable. So pure CPU software has two independent supports.

**The second fact also guarantees something the first cannot: the two stacks can mix, but they never merge.** Speed cannot carry this claim. A slow but specified component, such as a credit-bureau call that takes seconds, fits into the CPU stack like any other slow service. What cannot fit is judgment.

Wherever the stacks mix, a boundary remains. Post 2 named it a semantic boundary, alongside the speed boundary. Post 11 showed what vendors can do there: "Vendors can enforce what crosses the edge. They cannot tell code whether the judgment inside it was right." Post 24 summed it up: "The program unifies reaching. It does not unify trusting."

## Three categories

Two stacks give three kinds of software, and no fourth. None of them is new to this series. Each gathers patterns that Posts 2 and 3 already described.

- **C1: pure CPU.** You build code. The CPU runs it.
- **C2: pure LLM.** You build language (instructions, knowledge, skills), plus supporting code. The LLM runs it.
- **C3: mix.** You build code, language, and the boundary between them. Both run it.

### C1: pure CPU

AI's judgment is used at build time, and its result is written into code. As Post 2 noted, code an agent writes "can be inspected, compiled, tested, reviewed, revised, and approved before it becomes part of the executing system." At run time, nothing crosses between the stacks. An app written with a coding agent is still C1, because what you build is code, and the CPU runs it.

### C2: pure LLM

The LLM does the business work. Code, if you write any, only supports it. What you build is language: instructions, knowledge, skills. Supporting code may come with it, such as the loop that runs the model, the context it is given, the wiring for its tools, and the parsing of its output. Post 30, following Post 15, called the software around the model the harness. Whether a vendor wrote it or you did, it is supporting code.

Post 3's first scene has this shape: a research agent assembles a report, and a person reads it. As Post 3 put it, "Code moved the data around; it never had to understand the report." Post 0 said the LLM stack's counterpart of a running program "has no settled form yet." C2 is where that form is being worked out.

### C3: mix

Both stacks carry part of the business function, and judgment crosses into business logic. Post 3's other two scenes are C3: an agent decides a refund is justified and calls the refund code; a pipeline asks a model whether a deployment is safe and must act on the answer. C2 has a seam too, between the model and its supporting code, but nothing on the code side carries business logic. So C3 is the only category in which you have to design the boundary between judgment and business logic.

## Three rules

Three rules make the categories usable.

### 1. Purity is judged by what carries the business function, not by what hosts it

The LLM runs on CPUs and GPUs without joining the CPU stack, and supporting code hosts the model the same way. To tell supporting code from business logic, ask of any piece of code: would it still mean something to the business if the model were removed? A loop that runs the model, a prompt template, or an output parser would not. A pricing rule, a ledger update, or a refund check would. Then:

- No model at run time: C1.
- A model at run time, with only supporting code around it: C2.
- A model at run time, plus business logic in code: C3.

### 2. Inside C3, ask who owns the loop

Who owns the loop is one of the six questions Post 2 asked of any system that "uses an agent." In code-led C3, code owns the loop and calls the LLM for one step, such as extraction, routing, or review. In LLM-led C3, the LLM owns the loop and calls code as tools: an agent acting on business systems. The two can nest, as when a code-led process hands one of its steps to an LLM-led loop.

### 3. The categories sort what you build and own, not whole systems

Post 3 said of its three scenes: "A single application can contain all three scenes at once." A business system can hold C1, C2, and C3 parts side by side. It always holds people, too. Software cannot answer for its results, and the LLM stack "takes duties, not accountability" (Post 0). Someone in the business answers for every result.

## What each category asks of you

Each category has its own core problem:

- **C1:** code arrives faster than anyone can own it.
- **C2:** its business work is not specified, so it can only be checked by evaluation or by people.
- **C3:** the boundary, where judgment crosses into business logic (Posts 1–3 and 11).

The three categories ask for different work.

- **C1 asks you to own code you did not write line by line:** review that keeps up, integration that holds, and code that someone can maintain after its author moves on (Post 29).
- **C2 asks you to evaluate.** No test can certify it. What checks it is evaluation across many cases, and people who read the output. The bar is rarely exactness. For much of a business's work, close enough to be useful is the standard, as it is for advice from a person. So the check looks like spot-checking a junior colleague's work, not like testing code.
- **C3 asks you to design the boundary:** who owns the loop, what crosses it, and what limits a wrong judgment. Code can check the shape of what crosses; it cannot check the judgment behind it (Post 11).

My reading: most enterprise software will stay in C1, because both facts hold it there. C3 is where the hardest new engineering sits. C2 is where the LLM stack's programs are taking shape.

## Where this series goes next

Post 28 split agentic AI work into two sides, working on it and working with it, and this series took the "with" side. Post 28 named three areas on that side: software engineering, business operations, and science and research. It expected most readers to work in the first two.

The three categories cover all software built on the "with" side. The next step is a choice made for those readers, not a result of the reasoning above. From here, the series narrows to **working with AI in business**: enterprise software engineering and business operations. It leaves aside science and research, and specialized areas such as embedded and safety-critical code, PLCs, and games.

Seen through the run question, most of Post 31's findings are about C1. Post 33 will take all three categories through real examples, with analysis. For C2 and C3, that calls for newer evidence than Post 31 gathered.

After that, the series will turn to how each category is built: an AI-native SDLC for C1, evaluation for C2, and boundary design for C3. That is where the responsibilities Post 30 named for our side will get their turn: requirements, context, acceptance criteria, and operating outcomes. Across all three, one question will recur. What changes when AI is added to work that otherwise stays the same, and what changes when the work around it is reshaped?

**Before you build, ask what will run it: the CPU, the LLM, or both.**

---

## Earlier in this series

All posts are archived in the GitHub repository: <https://github.com/young-rex/notes-on-agentic-ai>

- Post 31: *AI Coding Has Two Sides. Ours Is Too Big for One Post.* <https://lnkd.in/p/gpS8XrPn>
- Post 30: *AI Coding Has Two Sides. First, Theirs.* <https://lnkd.in/p/gyg_buW9>
- Post 29: *Calibrate: Follow the Business Value*. <https://lnkd.in/p/gKBZHfWa>
- Post 28: *Calibrate: Working on Agentic AI, or Working with It?* <https://lnkd.in/p/gS5s4eRD>
- Post 24: *In the Future, Objects, Agents, and People Will Live in the Same Program*. <https://lnkd.in/p/gfgHcrgm>
- Post 15: *Calibrate: Where Does the Capability Live?* <https://lnkd.in/p/g7Es6PC3>
- Post 11: *The Models Are Getting Better. Code Still Cannot Fully Check Their Judgment.* <https://lnkd.in/p/gQqVvCsF>
- Post 10: *The CPU Stack Runs on Specification. The LLM Stack Runs on Judgment.* <https://lnkd.in/p/ga3apNfd>
- Post 3: *Does Software Ever Need to Call an Agent?* <https://lnkd.in/p/g822ukCZ>
- Post 2: *Agents Break Open Software's Stable Relationship*. <https://lnkd.in/p/gVtniMsP>
- Post 1: *Write Software That Uses Agentic AI*. <https://lnkd.in/p/gmh8qr5J>
- Post 0: *This series has formed a vision.* <https://lnkd.in/p/gZR-S3KV>
