# Some Work Can Be Spelled Out. The Rest Has to Be Trusted to Someone.

Friends asked why I wrote about IT hiring when this series is about agentic AI and enterprise AI transformation. Fair question. That post does not mention agents, models, or transformation. That was deliberate. But I never said so, and this article is the explanation I owe.

## The question every post asks: the work, and who holds it

Every post in this series asks one question about two things.

About the work: **which half of this can be specified, and which half requires judgment?**

About the participants: **who holds each half, and can they answer for it?**

These are not two subjects. Specification is what a system or a process can hold; judgment is what only a participant can. And the second question presupposes the first: you cannot ask who holds each half until the halves are named. That is why the two run together rather than side by side. Every post in this series is one instance of the same question: which half is this, who is holding it, and can they answer for it.

## About the work: an old line

I did not invent that line. Independent traditions have been drawing it for more than a century, each naming it in its own vocabulary, largely without reference to one another.

| Tradition | Name for the specifiable half | Name for the judgment half | When |
|---|---|---|---|
| Scientific management<br>(Taylor) | the specified method<br>motion study | retained by management<br>unnamed | 1911 [1] |
| Management<br>(Drucker) | manual work | **knowledge work** | 1959 [2] |
| Decision theory<br>(Simon) | programmed decisions | non-programmed decisions | 1960 [3] |
| Information systems<br>(Gorry & Scott Morton) | structured | unstructured<br>semi-structured | 1971 [4] |
| Selection research | the structured interview | interviewer judgment | 1997 [5] |
| Process modeling<br>(OMG) | BPMN — the flow you can draw | CMMN — the case you cannot | 2011, 2014 [6] |
| Agentic AI | the harness<br>the schema at the edge | the weights<br>the choice inside | 2023 [7] |
| Software engineering | the specification<br>`expected == actual` | *no name* | — |

Two cells in that column offer no name. They do not mean the same thing. Taylor had the judgment half and left it unlabeled; software engineering did not have it at all.

Software engineering has no established name for the judgment half because for most of its history it did not need one. If the work could be fully specified, the rest of the chain followed: contracts, automation, abstraction, standardization, composition, verification. An earlier post in this series traced that chain. The discipline had no word for the unspecifiable part because it had arranged not to have any.

## About the participants: a new kind

Before AI, the enterprise ran on two kinds of participant: people, and the deterministic systems they built. The division of labor was settled, even if nobody wrote it down. Systems held what could be specified. People held the rest, and answered for it.

A third participant has now joined, and management discipline has not finished deciding what it is.

| Participant | In this role for | Holds the specifiable half | Holds judgment | Can answer for it |
|---|---|:---:|:---:|:---:|
| Human | always | yes | yes | **yes** |
| Deterministic system | ~75 years | yes | no | no |
| Agentic AI | ~3 years [7] | partly | **yes** | **no** |

Trust has always required two conditions: that the holder can do the work it was handed, and that the holder can answer for the result. The last two columns are those conditions. Only the first row satisfies both, and it is the only row that ever has.

Trust never meant leaving judgment unexamined. A company trusts an engineer's judgment in the code they write and still builds review, tests, and deployment gates around the work. Those controls make judgment governable; they do not replace the person who can answer for it. With a model, the judgment remains, but the model cannot answer for it. The surrounding organization must.

The model is a new kind: the first participant that can perform judgment and cannot answer for it. Trust requires both columns, so this is not a kind that can earn it. The enterprise wants that judgment anyway, and is not wrong to. That is why management discipline has not finished deciding who answers for it.

## Hiring: the control case

There is no AI in the hiring post. I left it out on purpose, and the failure it describes needed none.

That is what makes it a control case: take the newest participant out of the picture and see whether the failure still happens.

It does. An applicant tracking system is a deterministic system — the middle row of that table. It can hold the specifiable half of hiring: whether an application carries the required certification, the minimum years, the right keywords. It cannot hold the judgment half, which is whether this person can do the job.

Enterprises handed it the judgment half anyway. Four hundred applications, zero responses. The process executed perfectly. It did not evaluate. The system could not answer for that outcome, and no person did either.

Both conditions of trust failed, and keyword matching and rule engines were enough to fail them. Judgment displaced onto a holder that cannot carry it is an old enterprise failure — not something AI introduced.

## What this does and does not settle

The question tells you where to look — which half is this, who holds it, can they answer. It does not tell you what to build. I have argued that organizational redesign has to become operational rather than advisory, and I do not yet have a method for that.

The hiring post was not a detour. It asks the same question as every other post, in a case with no AI in it. That question is the ground this series stands on. An enterprise that cannot answer it in hiring, where no AI is involved at all, has little reason to expect it will answer it with one.

---

## Appendix: how to read this series

Since this is the first article about the series itself, a few conventions are worth stating.

**It is written in a semi-social-academic style.** The audience is professional, not a journal. Sources are used to establish facts, and the facts must be right — a citation here should say what I claim it says. The conclusions I draw from those facts often reach further than any single source supports. Where a reading is mine rather than the source's, I try to mark it in the text.

**References are external sources only, in a numbered academic block.** Author, title, publication, year, and a link — plus, where it matters, a sentence on what the source actually establishes and where its limits are.

**Earlier posts in this series are never cited as references.** They are referred to in prose and listed separately below. A post of mine is not evidence for another post of mine, and putting one in a numbered block would dress an argument up as a source.

**Where to find the series.** Once a series runs past a handful of posts, it is hard to see it whole on LinkedIn. The GitHub repository is the index — every post, numbered, in full text. See the screenshot below.

<https://github.com/young-rex/notes-on-agentic-ai>

## References

[1] Frederick Winslow Taylor, *The Principles of Scientific Management*, Harper & Brothers, 1911. Taylor develops scientific management by assigning method design, work study, selection, and training to management while workers execute standardized methods. The table's placement of residual judgment with management is an interpretation of that division; Taylor does not name a separate "judgment half." <https://www.gutenberg.org/ebooks/6435>

[2] Peter F. Drucker, *Landmarks of Tomorrow*, Harper & Brothers, 1959. Identifies the rise of knowledge work and the knowledge worker in contrast with manual work. The table's alignment of knowledge work with the judgment half is this article's synthesis, not a taxonomy Drucker states in these terms. <https://search.worldcat.org/title/387964?page=citation>

[3] Herbert A. Simon, *The New Science of Management Decision*, Harper & Brothers, 1960. Distinguishes programmed from non-programmed decisions. The table maps that distinction onto specification and judgment; those are this article's umbrella terms rather than Simon's. <https://openlibrary.org/works/OL1205032W/The_new_science_of_management_decision>

[4] G. Anthony Gorry and Michael S. Scott Morton, "A Framework for Management Information Systems," *Sloan Management Review* 13, no. 1 (Fall 1971): 55–70. Combines management levels with structured, semi-structured, and unstructured decisions to frame information-system support. The two-column table compresses their three-part continuum into the two halves used by this article. <https://dspace.mit.edu/bitstream/handle/1721.1/47936/frameworkformana00gorr.pdf>

[5] Michael A. Campion, David K. Palmer, and James E. Campion, "A Review of Structure in the Selection Interview," *Personnel Psychology* 50, no. 3 (1997): 655–702. Identifies fifteen components of interview structure and evaluates each against reliability, validity, and user reactions. The paper’s own division is between components that enhance the interview’s content and those that enhance its evaluation process; the table’s split between the structured interview and interviewer judgment is this article’s compression, not the authors’ framing. Julia Levashina, Christopher J. Hartwell, Frederick P. Morgeson, and Michael A. Campion, "The Structured Employment Interview: Narrative and Quantitative Review of the Research Literature," *Personnel Psychology* 67, no. 1 (2014): 241–293, consolidates the later literature. <https://doi.org/10.1111/j.1744-6570.1997.tb00709.x>

[6] Object Management Group, *Business Process Model and Notation (BPMN), Version 2.0*, January 2011; and *Case Management Model and Notation (CMMN), Version 1.0*, May 2014. BPMN specifies a notation for modeled business processes; CMMN specifies one for case work whose activities may unfold in an unpredictable order as circumstances evolve. Calling them "the flow you can draw" and "the case you cannot" is a deliberately compressed contrast, not language used by the standards. <https://www.omg.org/spec/BPMN/2.0/PDF> · <https://www.omg.org/spec/CMMN/1.0/PDF>

[7] OpenAI, "New Models and Developer Products Announced at DevDay," 6 November 2023. Introduced the Assistants API as a first step toward agent-like experiences, combining instructions and additional knowledge with model and tool calls. It is used here as a practical product-era marker for agentic AI, not as a claim that agentic AI began on a single date. <https://openai.com/index/new-models-and-developer-products-announced-at-devday/>

## Earlier in this series

- 19 IT Hiring Is Broken Because Process Outranks Judgment.md
- 18 Enterprise Systems Before AI Academia Vendors Architecture Process Data and Operations.md
- 13 Govern the AI, and Redesign the Work.md
- 12 The Enterprise Chose Speed. Own the Burden.md
- 10 The CPU Stack Runs on Specification. The LLM Stack Runs on Judgment.md
- 08 Knowing Every Layer Is the Job. Building Every Layer Is Not.md
- 06 AI Takes Duties, Not Accountability.md
