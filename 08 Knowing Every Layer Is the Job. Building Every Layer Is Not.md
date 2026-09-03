# Knowing Every Layer Is the Job. Building Every Layer Is Not.

*Originally published on LinkedIn: <https://lnkd.in/p/gReNQhZ2>*

The last post put the LLM in the processor slot and asked what has grown around it. The answer was: a stack that remains early and unsettled. Six rungs the CPU stack climbed over decades — portable languages, paradigms, the split between systems and application programming, information systems as its own discipline, methodology, execution models — and the LLM is on the first one, barely.

That was the diagnosis. Here is the response, stated before the argument for it: an enterprise cannot wait for the stack to settle, and it should not try to build every missing layer itself.

## The relationship nobody has built

There is a third thing, and it is the least built of the three. Not the processor, and not the stack — the relationship between them and us.

For decades that relationship was a line. At the system-building layer, humans encoded intent in software and machines executed it, and every layer and method we added sat on that line. It is now a triangle — human, model, machine — with one edge decades older than the other two:

| Edge | Age | What crosses it |
|---|---|---|
| Human and machine | Decades | Software the human writes, on a stack nobody thinks about any more |
| Human and model | Current form: about four years | Natural language, in both directions |
| Model and machine | Current agentic form: about three years | Software the model writes, tool calls included |

The framing that misses this is "the LLM is a tool humans use." A tool sits at the end of somebody else's edge. This one has an edge of its own: it issues calls a harness executes against the machine, and it runs in the first place because that harness invoked it. Participant here means exactly that and nothing more — something with edges running both ways, not something with a claim on judgment or blame. The model-to-machine edge now carries something once confined to the human-to-machine edge: software.

So what are those relationships? Who calls whom, who waits, who reviews, who answers for the outcome?

One part of that is already settled. The model cannot answer for the outcome — it has no way to hold accountability, and no share of it ever transfers. What stays unsettled is which human does, with what authority, what visibility, and what control.

And on that we cannot say much yet, for a reason that is not lack of thought. **Relationships get their teeth from contracts; system-level contracts need stable interfaces to attach to.** An organization can assign accountability by policy today, and process can enforce it case by case. But repeatable, auditable enforcement at system scale needs control points, and control points need interfaces. Those interfaces are still churning. The unfinished stack is one reason the triangle remains unfinished.

That sounds like a reason to wait. It is not. The reason is an old move in the information systems discipline.

### That was never our job

The information systems discipline does not typically build processors, operating systems, languages, or compilers. That is not a lack of interest. It is a division of labor. Those are tools that serve the enterprise, built by people whose job is to build them.

It is worth being precise here, because these are three different disciplines and the difference is easy to miss. The distinguishing question is the unit of analysis.

| Discipline | What it studies | The same subject: databases |
|---|---|---|
| Computer science | The computation — what can be computed, at what cost, with what guarantees | Relational algebra, query optimization |
| Software engineering | The construction — how to build and maintain large software reliably | Schema design, migration, object-relational mapping |
| Information systems | The organization — how people, process, and technology fit together to serve a purpose | Data governance, master data management, who owns the customer record |

Three disciplines, one subject, three entirely different questions — and the third is often taught in business schools rather than engineering faculties because its unit of analysis is the organization, not the program.

And the third one has never waited for the stack to be finished. It has contained whatever stack existed at the time — at every rung of that ladder, without waiting for the next one.

When mainframe memory was measured in kilobytes, we ran payroll and inventory anyway, fitting the business process to the machine's limits rather than waiting for better machines. Before object orientation became mainstream, we built large systems with modular decomposition, data dictionaries, and systems-analysis methods — containment invented before the language offered it to us. When distributed computing had no unified abstraction, we combined local object models, network protocols, service contracts, and organizational practices — and shipped.

That move deserves a name: **composition under an absent abstraction.**

The last post showed the same condition inside today's agent harness: state, tool dispatch, stop conditions, steering messages, follow-ups — hand-assembled in one file, because no settled abstraction supplies them. Even the people closest to the model are composing under an absent abstraction.

It is the oldest move in the enterprise discipline's repertoire. It is also exactly the one this moment calls for.

### The ratio nobody mentions

There is a reality behind this that is easy to miss.

Count the jobs at the layer that gets written about — kernels, compilers, chips. Those populations are tiny. The application and enterprise population is enormous.

That ratio is not a disappointment. It is what a mature stack produces: few people build the layer, many people use it.

Right now an unusual share of LLM engineering effort sits at the harness layer, hand-writing loops and memory and tool dispatch. That is not the permanent shape of the field. It is what a missing stack does to a labor market — and it is exactly what today's hand-built agent harnesses show you.

An enterprise can genuinely be behind — on adoption, on skills, on where its data sits. But not building a proprietary harness is not evidence that it is behind. That layer is likely to disappear into infrastructure, as the layers beneath it already have. The arrangement — who calls whom, who reviews, who answers for it — is the part that stays, and it is the part nobody has built.

Stack literacy means knowing what each layer guarantees, where state lives, how failures surface, and who owns the result — even when somebody else built the layer.

Track those layers as they mature. Know when to build, when to wrap, and when to wait.

Concretely: build where the workflow is what differentiates you. Wrap the unstable layers behind interfaces you own. Wait where the guarantees you need do not exist yet.

That literacy is the job. Building every layer yourself is not.
