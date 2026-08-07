# The CPU Stack Runs on Specification. The LLM Stack Runs on Judgment.

The previous post argued that when a stack is young, old problems get new names — and that recognizing a development arc does not tell you the arc ends in the same place. This post asks what made the first arc possible and why the same foundation does not carry the second.

## The chain that built the first stack

The CPU stack was not built by accumulated effort over time alone. It was built because the concerns at each layer shared a specific property: they could be deterministically specified. An instruction set defines exactly what each operation does. A memory model defines exactly which addresses a process can see. A type system defines exactly which operations are permitted on which data. A function signature defines exactly what goes in and what comes out. At every layer, someone could write a complete, precise contract for the behavior — and once written, it held.

That specifiability was not one property among several. It was the foundation the rest was built on. From it, a chain of capabilities followed, each depending on the one before.

Contracts came first. Because behavior could be specified, two layers could agree in writing on what crossed the boundary between them. The agreement could be stable, versioned, and enforced. When it broke, both sides could point to the document and say which one violated it.

Automation followed from contracts. A machine can perform a task without human involvement when the contract tells it exactly what to do. There is no gap in the specification that requires someone to step in and decide. This is why compilers work, why operating systems schedule processes, why databases execute queries — the behavior at each layer was specified completely enough that a machine could carry it out.

Abstraction followed from the same root. A layer above can ignore how the work is done below because the contract tells it what to expect. An application developer does not need to understand memory management because the operating system's contract promises to handle it. That promise is credible because the behavior is fully specified, not because the developer chose to trust it.

Standardization became possible because there was something precise to agree on. An industry can converge on a shared instruction set, a shared system-call interface, or a shared language specification because the behavior behind each one can be written down completely. Standards committees argue about what the specification should say, not about whether a specification can say it.

Composition followed from trustworthy contracts at each part. Larger systems could be built from smaller verified components because each component's contract was reliable. If a function promises to return a sorted list, the caller can build on that promise. If the promise holds, the composition holds. This is why libraries work, why frameworks work, why the stack itself could grow to the height it reached.

Verification closed the chain. Testing, type checking, and formal methods all require a specification to verify against. You cannot check whether behavior is correct without a definition of correct. The CPU stack had that definition at every layer. Verification was hard, but it was structurally possible — the target existed, and tools could be built to measure against it.

## A different foundation

Each of these capabilities traces back to the same root. Remove specifiability, and contracts become partial. With partial contracts, automation covers only the specifiable portion. Abstraction can hide the mechanics but not the unspecified decision. Standardization has less to agree on. Composition cannot guarantee what the unspecified part of each piece will do. Verification has no complete target to verify against.

That is exactly the condition the LLM stack is in.

The judgment that sits inside context selection, loop termination, and graph routing — the semantic decision this series has been examining since the first post — is not deterministically specifiable. It is learned behavior applied to open-ended input, and no contract fully defines what the model will conclude about a request it has not seen before. You can constrain the format. You can narrow the range of acceptable answers. You can wrap the output in deterministic checks. But the decision itself — was the right context selected, is the task actually done, which path fits this case — cannot be written down as a complete specification the way an instruction set or a memory model can.

This does not mean the LLM stack is failing. It means the foundation is a different material. The CPU stack was built on behavior that could be fully specified, and from that property every other capability followed. The LLM stack rests on behavior that cannot be fully specified — and that changes what the chain above it can deliver. Contracts are partial rather than complete. Automation covers the mechanical half but defers the semantic half. Abstraction hides the plumbing but leaves the judgment exposed. Composition works for the deterministic scaffolding around the model but not for the model's contribution within it. Verification can check structure and measure tendencies across cases, but it cannot certify that any single judgment was correct against a complete specification — because no complete specification exists.

## A different material

The difficulty, then, is not youth alone. A young stack matures with effort and time, climbing the rungs the CPU stack climbed before it. But the LLM stack's foundation has a property that the CPU stack's foundation did not: it resists the complete specification that every rung above depended on. That does not mean the LLM stack will fail to mature. It means it will mature differently — developing its own mechanisms, its own contracts, its own forms of verification, adapted to a foundation made of judgment rather than specification. What those mechanisms look like is an open question. That they cannot be the same ones is not.

---

## Earlier in this series

- [When a Stack Is Young, Old Problems Get New Names](Post 9)
- [We Built the World Around the CPU. We Are Doing It Again Around the LLM.](Post 7)
- [Write Software That Uses Agentic AI](Post 1)
