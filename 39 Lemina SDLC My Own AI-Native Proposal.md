# 39. Lemina SDLC: My Own AI-Native Proposal

*Originally published on LinkedIn: <https://lnkd.in/p/gR-Uu5hA>*

Post 38 evaluated four AI-native SDLC proposals through the same four lenses: context, coordination, checks, and authority. It also disclosed that I have a proposed method of my own, Lemina SDLC, and promised that the next post would examine it through those lenses and compare it with some of the cases.

That evaluation needs Lemina on the page first. This post introduces Lemina SDLC: the stack it is designed for, its principles, the structure it proposes, and the aims that structure serves. The evaluation Post 38 promised moves to Post 40.

## 1. Foundation and design principles

Lemina SDLC is a structural guideline for AI-native software development. It defines how software and its artifacts are organized, not the processes that change them. Its name marks where it applies, the software development lifecycle; it defines no lifecycle of its own. Teams can embed it in, or combine it with, other AI-native SDLC methods or frameworks, which supply processes, actor assignments, governance, and tooling. It is AI-native in how it organizes software: it is designed for the LLM stack as Post 34 defines it, and making work digestible for AI shapes how the software is organized into micro-features and how their artifacts are grouped. It works with the documents and tools teams already use.

### 1.1 Foundation: the LLM stack

Post 34 defines the LLM processor as a model together with its hardware and serving software. Its one processor-level operation is the LLM call, which reads one context and generates one response. The agent call is built on top of it: a harness loops through LLM calls and tool calls until the model gives a final response or the harness stops it. Callers reach both by passing messages. In this document, AI means this stack: LLMs and the agents built on them.

Three characteristics of this model matter for organizing software work:

- **Semantic judgment on language.** An LLM is told what to do in language and responds by judgment rather than by specification (Posts 10 and 34). Its language includes prose and code, so AI can work with requirements, designs, source code, and tests alike.
- **Selected context within a finite window.** Each LLM call reads only the context its caller selects and composes for it (Post 34), and that context must fit within the LLM processor's finite context window.
- **Task state through context.** The LLM processor keeps no task state for its callers; each call gets what it needs from its context, and continuity between calls comes from the harness and from external stores that tools reach (Post 34). Work that recurs over a project's life, across many agent calls and changes, needs durable records.

Lemina's principles respond to these characteristics through how the software and its artifacts are organized, rather than by prescribing a harness or a process; work on the software takes its shape from that organization.

### 1.2 Three principles

The order of the three principles follows the design rationale: digestibility drives the organization of software into micro-features, and both motivate bringing each micro-feature's artifacts together.

1. **Make the work digestible for AI.** An agent call can span many LLM calls, but each call can consider only the context selected for it within a finite window. That is why size matters: smaller work units help keep the information needed for each judgment within that window. A role's task should therefore be manageable and its relevant context identifiable. Following Post 13, shape work units so the judgment inside each is small, its output checkable, and the effects of mistakes contained. In size, aim for work units on which AI can do a role's task within one agent call. Lemina pursues digestibility through structure: it guides the sizing of micro-features (§2), which in turn helps keep the work done on each manageable.

2. **Organize the software into micro-features.** Because task state reaches the processor only through context, work repeated across agent calls and changes needs records outside it. Lemina organizes the software, and those records, into micro-features: lasting spaces in the software's structure, each meant to hold a unit of its functionality. Their persistence lets later work, in another agent call or another change, return to the same micro-feature and its records. Micro-features may compose hierarchically into larger ones and are refined progressively (§2).

3. **Bring each micro-feature's artifacts together.** Digestibility and the micro-feature structure motivate keeping all artifacts of a micro-feature together (documents, source code, and tests alike) as one versioned artifact set. The set forms one conceptual unit that can be understood and reviewed together. Because AI works on prose and code alike, it can examine that unit's requirements, specifications, designs, code, and tests together, across artifact types. Group these durable artifacts to help actors (as in Post 37, whoever performs an activity, person or agent) identify and supply the evidence a role's task needs, for production and for verification; that evidence may also include parent, sibling, or shared artifacts.

Lemina states the principles and their design intent; the team does the creative and practical work of applying them. Lemina defines structure, not process, and enforces nothing: how work moves through the structure, who performs and approves it, and which tools check it are decisions for the surrounding method and the team.

Section 2 describes the structure Lemina provides: micro-features as building blocks. Section 3 describes the strategic aims that structure is intended to serve.

## 2. Micro-features as building blocks

Lemina's structure rests on one concept: the micro-feature. This section describes what a micro-feature is, what it holds, how micro-features compose, why composition is meant to support larger applications, and how a project adopts the structure. It closes with what Lemina leaves to the surrounding method.

### 2.1 A space for a building block

A micro-feature is a persistent, size-guided space in the software's structure, meant to hold one unit of the software's functionality.

Lemina fixes the shape of that space:

- **A place in a tree.** Micro-features form a tree whose root is the project.
- **Room for a full artifact set.** A micro-feature holds the artifacts that describe and implement its functionality.
- **One versioned unit.** The set changes as a whole, version by version.

The team decides what fills the space: which functionality, where the boundaries fall, and which artifact types the project uses.

*Persistent* means a micro-feature lasts as long as the functionality it holds; unlike a task or a ticket, it remains for later work to return to. *Size-guided* means its boundaries respond to the size of the work and context AI must handle, rather than to a particular programming language, architecture, protocol, or number of code lines. The target is digestibility (§1): within one agent call, AI can do a role's task on the micro-feature, and an agent can examine the micro-feature's own artifact set with relevant surrounding context. An agent call can read the set across many LLM calls, so the whole set need not fit in one context window. Digestibility is a design target, not an entry condition: a space can exist before it reaches the target, and the target moves with the models and agents in use.

The micro-feature sits between two familiar concepts. Like a feature, it holds a unit of function; like a class, it is a persistent part of the software's structure. The relation runs both ways: a micro-feature's code may contain many classes, and one business feature may span several micro-features. Despite the shared prefix, a micro-feature is not a microservice: it implies no deployment unit, and Lemina assumes neither that it owns data nor that it exposes an interface for others to call. Where data or interactions matter, its artifacts describe them, usually in prose.

The class is also the precedent for treating the micro-feature as a space. A programming language fixes the shape of a class, and nothing forces the programmer to fill it with one coherent responsibility. Classes became building blocks anyway, because the construct made good structure possible and design practice made it common. Lemina proposes the micro-feature in the same spirit: the space makes a useful building block possible, and the team's design makes it one. Lemina therefore offers no formal test of whether a given space is a good micro-feature; that judgment depends on the models and agents in use and belongs to the team.

### 2.2 The artifact set

A micro-feature's artifact set holds the artifacts that describe and implement its functionality: requirements, specifications, designs, source code, and tests, as the project defines them. Source code and tests belong to the set as fully as documents do; documents will usually be prose. The project decides which artifact types it uses, and the subsection on adoption below describes what a missing type costs.

The set is kept in one place, such as a Git repository, so that it can be read and examined as one unit, and every artifact belongs to an identifiable micro-feature. Folder structure, naming, and formats remain project choices.

The set is versioned as a unit. Its current version is the latest accepted state, the one later work reads, for example to prepare an agent's context; being current does not prove an artifact correct. A change produces a new version, and a pull request (PR) is one way to propose one: applying a PR's changes to the current version yields a candidate version. Verification concerns that resulting state, including relevant unchanged artifacts, so a change that touches only source code is still examined against the existing requirements and specifications. That means judging the relevant artifacts as they will stand together, not rechecking everything on every change.

Where a host method produces artifacts for a single change, such as a specification for one feature request or a plan for one story, embedding Lemina means carrying what each change establishes into the persistent artifact sets of the micro-features it affects. Lemina describes that relationship; the host method decides how to maintain it.

### 2.3 Leaves and parents

Leaf micro-features hold concrete implementation: source code and tests, together with the documents that describe them. Parent micro-features build on their children, which may be leaves or other parents.

A parent's artifact set holds its own artifacts, not its descendants': typically a requirement and specification for the combined behavior, a design explaining how the children's contributions fit together, shared constraints, and tests of the combination. A parent organizes its descendants rather than containing them; otherwise the root would contain the entire application.

Specifications are the route to composition. An actor can understand a parent through its own artifacts and its children's specifications, without reading all of their source code, and can reach a child's implementation artifacts when deeper investigation needs them. For understanding, a child's specification plays the role an interface plays for a class, but in prose rather than as callable signatures. Integration code that connects children may sit in the parent or in an appropriate leaf, as long as its location and relationships are understandable.

For example, refund processing might be a parent with three children: eligibility assessment, amount calculation, and refund recording. The parent's artifacts state the end-to-end refund behavior, the shared policy (for example, that a refund never exceeds the original payment), how the three steps fit together, and tests of the whole flow. Each child holds its own specification, design, code, and tests. An agent examining refund processing reads the parent's artifacts and the three children's specifications; an agent working on amount calculation reads that child's full set together with the parent's shared policy. The business feature "refunds" spans three micro-features, and the code for amount calculation may contain several classes.

### 2.4 Composing larger applications

Small units alone explain how software can be divided; the specification hierarchy explains how it can be built up. At each level, children's specifications stand in for their implementation detail. An actor reasons about a parent through its children's specifications, then examines an individual child with the relevant parent and dependency context. Repeating this arrangement across levels is intended to keep the detail each judgment needs manageable as the application grows, which is what a finite context window calls for (§1). The tree also keeps durable records findable: later work, in another agent call or another change, goes to the relevant micro-feature instead of searching the whole application.

Composition through specifications works only while each specification matches the implementation it stands for. Here Lemina's two central ideas meet: specifications enable composition, and semantic consistency checks, made practical by the grouped and versioned artifact set, help assess and maintain their alignment with implementation (§3). Together, they are how micro-features are meant to support larger applications.

This benefit has limits:

- **The hierarchy alone guarantees nothing.** Its effectiveness depends on the team's boundaries, its specifications, and how well dependencies are represented.
- **Some detail does not shrink.** A task may involve many siblings, dependencies across branches, or constraints that apply to the whole application, such as security and performance. Moving shared concerns to a common ancestor organizes them; it does not necessarily make them small.
- **Checks are judgments.** They can miss inconsistencies, and they cannot catch what is consistently wrong: code, specification, and tests that agree on the wrong behavior. That takes evidence of intent from outside the set, such as stakeholder validation.
- **The full set is not the context.** Keeping the full set available does not mean putting every artifact into every AI call.

### 2.5 Adoption and AI

Lemina describes an ideal arrangement without requiring a project to reorganize at once. An existing project can start as a single root micro-feature holding everything; the team then introduces and refines micro-features over time, designing boundaries, observing the results, and adjusting. When a micro-feature grows past the digestibility target, for example when an agent can no longer examine the micro-feature's own artifact set within one agent call, that is a signal. The response is the team's: splitting it, simplifying it, or waiting for models that handle more.

Lemina's proposition is that AI assistance can make this richer artifact arrangement practical by reducing the effort of drafting and updating requirements and specifications alongside code. Maintaining such documents for many small units is long-standing good practice, but the effort has often exceeded what teams can sustain, so the documents tend to go stale. AI can also help decompose an existing application into micro-features (§3) and draft each one's artifacts.

Specifications AI derives from existing code describe what the code does, bugs included, not necessarily what the business intended. One possible path: AI drafts specifications from the code and compares them with independent evidence, such as tests, user documentation, tickets, and the specifications of sibling and parent micro-features. It reports discrepancies and uncertainty, and people confirm the intent where it matters. Repeated rounds may narrow what remains for people to resolve, and teams can take other paths.

Requirements and specifications belong to the ideal arrangement. Progressive adoption accommodates projects that lack them, at a cost:

- **Without specifications,** actors lose the specification-based route to understanding composition and may need code or other evidence instead.
- **Without requirements,** consistency checks lose an explicit reference for business intent within the set; other evidence or stakeholder validation may still establish it.

### 2.6 Outside Lemina

Lemina defines no processes. The surrounding method and the team decide how changes are proposed and accepted; who performs, reviews, and approves work, and which of those actors are people or agents; what verification runs and whether it blocks a change; and which tools carry these decisions out. Authority and accountability follow the organization's existing allocation.

## 3. Strategic aims

Section 1 describes Lemina SDLC's principles and §2 its structure. This section describes the strategic aims that structure is intended to serve: sustained semantic consistency, manageable development as projects grow, and AI-assisted decomposition and construction. These aims guide Lemina's development and evaluation. They introduce no practices, and their effectiveness in Lemina has not yet been demonstrated.

### 3.1 Sustained semantic consistency

The aim is sustained, systematic semantic consistency across each micro-feature's artifact set. It builds on two parts of the arrangement: AI works on prose and code alike (§1), and each micro-feature's artifacts are grouped as one versioned set (§2). People or agents, such as a review agent, can then examine a micro-feature's requirements, specifications, designs, source code, and tests together for contradictions, omissions, and drift. For example, a check is meant to flag code that permits a refund the requirement and specification disallow. Checks can compare only the artifacts a project contains, so their value depends on its artifact types (§2).

Such checks have two uses:

- **On a candidate version,** they compare the proposed changes with the relevant artifacts, changed or not.
- **On the current version,** they examine an existing micro-feature as a whole, continually or at regular intervals, including artifacts no recent change has touched.

A model-judged finding rests on judgment (§1), so it is evidence for actors to assess, alongside decisive checks such as compilation and tests. Checking is also intended to reach beyond individual changes, bringing to light an artifact that should have changed but did not. Findings lead to corrections through whatever change process the team uses.

### 3.2 Manageable development as projects grow

Composition through specifications (§2) is meant to let a large application keep its tasks and their relevant context digestible as it grows: each level is understood through its own artifacts and its children's specifications, and shared constraints sit with the parent that organizes them. A change that spans many micro-features remains a larger change, touching several artifact sets; §2 lists where the hierarchy's help runs out.

### 3.3 AI-assisted decomposition and construction

Once a project is organized into micro-features, the micro-feature becomes a unit the team can design with. Teams may then use AI to decompose a project, or new work within it, into micro-features and to construct the software from those units: proposing units, their place in the hierarchy, and the artifacts each needs. The aim resembles object-oriented programming: once classes and objects were introduced, programmers began to construct programs from them. The comparison concerns design practice, not mechanism; a micro-feature is a space that Lemina defines and practice fills (§2), not a language construct.

Teams may decompose one level at a time, splitting a parent micro-feature into its children, so that each decomposition task is itself intended to stay digestible. A decomposition AI proposes remains a proposal: the team designs and judges the boundaries (§2), and adopting it changes artifact sets through whatever change process the team uses.

---

## Earlier in this series

All posts are archived in the GitHub repository: <https://github.com/young-rex/notes-on-agentic-ai>

- Post 38: *Evaluating AI-Native SDLC: Four Cases Through the Same Four Lenses* <https://lnkd.in/p/gRgZVnD4>
- Post 37: *AI-Native SDLC: The Landscape* <https://lnkd.in/p/gkDevySp>
- Post 34: *Defining the LLM Stack: One Operation and Messages* <https://lnkd.in/p/gBRCT5R6>
- Post 13: *Govern the AI, and Redesign the Work* <https://lnkd.in/p/g-uNHNR7>
- Post 10: *The CPU Stack Runs on Specification. The LLM Stack Runs on Judgment.* <https://lnkd.in/p/ga3apNfd>
