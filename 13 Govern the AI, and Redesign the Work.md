# Govern the AI, and Redesign the Work

The last post closed a question. The enterprise chose speed. Speed made per-output human review impossible. The judgment that code cannot check and humans can no longer review needs an owner — and that owner is the enterprise that chose speed.

Now what?

Two directions exist. One is well underway. The other is increasingly discussed, but only a small minority report making meaningful progress. The enterprise needs both.

But before the directions, I want to say what I think we are witnessing — because it determines whether you see agentic AI as a technology problem or as something larger.

## A new participant joined the workforce

This is my framing, and I have not found it elsewhere.

Every time a fundamentally new type of participant entered the workforce, management theory evolved. Not because someone decided to update it, but because the old methods stopped fitting.

**Chapter 1: The manual worker.** Taylor published *The Principles of Scientific Management* in 1911, as large industrial enterprises confronted the problem of coordinating work at scale [1]. Scientific management divided planning from execution: management studied and specified the method; workers were selected, trained, and supervised to perform it. Motion and time studies became emblematic. The worker's contribution was treated primarily as execution, while management retained planning and measurement.

**Chapter 2: The knowledge worker.** Drucker identified knowledge work and the knowledge worker in 1959's *Landmarks of Tomorrow* [2]. A different type of contribution was becoming economically central — work whose primary resource was knowledge rather than manual execution. Industrial command-and-control methods did not transfer unchanged. Across *The Practice of Management* and his later work, Drucker developed management by objectives and self-control, decentralization, autonomy, and self-management [3]. Management's job shifted from specifying motions toward setting objectives and enabling the worker's judgment.

**Chapter 3: The AI worker.** This is happening now, and neither Taylor's nor Drucker's methods fully apply. This participant carries duties (Post 4) but returns no accountability (Post 6). You cannot prescribe its motions — its behavior is learned, not specified (Post 10). You cannot trust its judgment the way Drucker trusted the knowledge worker's — because it cannot answer for what it concludes (Post 6), and code cannot check whether its judgment was right (Post 11).

Each chapter was triggered by the same event: a participant entered whose nature differed from the previous one, and management had to evolve to accommodate what it actually was. We are at the start of the third.

Early evidence supports the pattern. Marta Stelmaszak, Mayur Joshi, and Ioanna Constantiou describe AI as organizing "new structures, roles, expertise, and organizational practices of the participating human and algorithmic actors" — a reconfiguration, not an addition [4]. Wiles and her coauthors test a related question directly: does labeling the same producer as an AI tool, an AI employee, or a human employee change how managers review the work and assign accountability [5]? That experiment exists because this participant does not fit existing categories cleanly.

The distinction that makes Chapter 3 different from an extension of Chapter 2 is the one this series has been building: AI takes duties, not accountability. The knowledge worker could answer for their judgment. The AI worker cannot. That is a new property in the workforce, and it is why existing management methods — whether Taylor's or Drucker's — do not transfer unchanged.

## Direction 1: Govern the AI

The industry is building governance infrastructure at pace. NIST AI RMF and ISO/IEC 42001 provide two widely used governance anchors [6]. NIST offers a voluntary risk-management framework; ISO/IEC 42001 specifies requirements for an AI management system. Monitoring platforms detect drift, flag anomalies, and log decisions for audit. Guardrails constrain what agents can do. Deployment gates control what reaches production.

This is real and necessary.

Alongside the infrastructure, a debate is running about what AI is to the organization. Research reported in *Harvard Business Review* warns against treating agents like employees: employee framing reduced individual accountability and review quality while increasing unnecessary escalation [7]. Microsoft's 2025 Work Trend Index reports that 28% of managers are considering hiring "AI workforce managers" to lead hybrid teams [8]. Mercer's 2026 Global Talent Trends reports that 82% of C-suite respondents see the future of HR as managing human talent and digital agents side by side, while 63% rank redesigning work for AI and automation as their highest-ROI people priority [9].

The debate matters. It also shows that work redesign is entering the mainstream conversation. What remains limited is demonstrated execution at depth.

Deloitte's finding sits underneath: only 6% of leaders say they are making progress designing human–AI interactions [10].

Governance is necessary. It is not sufficient. The enterprise also needs the second direction.

## Direction 2: Redesign the work

This is where I want to push the current conversation further — from advocating redesign in principle to specifying the unit and sequence of redesign.

The industry has built more mature practices for governing AI than for redesigning work around it. The enterprise must do both — not one after the other, but alongside each other. Governance becomes easier, and some controls become unnecessary, when the work itself is shaped for the new participant.

This series has been building the pieces:

**A role is a bundle of duties. Duties are the unit to allocate** (Post 4). The question is not whether AI replaces a role. It is which duties move, and which stay.

**When duties move, redraw the business process** (Post 5). The flows between roles — handoffs, reviews, decisions, controls — were designed for the old allocation. They do not automatically fit the new one.

**AI takes duties, not accountability** (Post 6). The accountability never transfers. Organizational design must absorb this.

**Code checks the edge, not the judgment inside** (Post 11). Duty design should account for this: how checkable is the output of this duty at the edge?

**The enterprise chose speed. Own the consequences** (Post 12). The enterprise that made that choice holds the five levers and must name who owns them.

These are not separate observations. They are a design sequence:

> **Decompose the role → Allocate duties by checkability → Reshape the work units → Redraw the process → Assign the levers.**

Two ideas follow from this sequence. They build on an emerging body of work-redesign practice, but the way I combine them at the duty level is my contribution.

### Manage AI duties with management discipline

The governance conversation treats AI as technology to be governed. The employee-vs-tool debate argues about the right metaphor. This series resolved it several posts ago: AI is neither tool nor employee. It is a new participant that takes duties but returns no accountability.

That means: apply management discipline — scope, supervision, review, escalation, performance measurement — to the duty, not to a role-holder who can answer back. The enterprise already knows how to manage duties. It does it for every human role. The difference is that the accountability loop never closes at the AI. It closes at whoever holds the levers.

This is not a new discipline. It is an existing discipline applied to a new participant — the same move Taylor and Drucker each made when their new participant arrived.

### Make work AI-digestible

This flips the question the industry is asking.

Everyone asks how to make AI fit existing work. I am asking the opposite: how do we reshape work to fit what AI actually is?

If AI is going to carry duties, the work units it receives should be designed for what it can do — scoped narrowly enough that the judgment inside is small (Post 3), structured so the output is checkable at the edge (Post 11), and decomposed so a wrong answer is containable rather than catastrophic.

This is not prompt engineering. It is organizational design — rethinking how work is broken down, handed off, and verified, so that the arrangement between people and AI is sound by construction rather than patched by governance after the fact.

The enterprise that makes its work AI-digestible reduces the burden it took on in Post 12. Not by governing harder, but by reducing how much governance must compensate for poor work design.

## Where this goes

This is a direction, not a finished method. I do not have a proven framework for duty-level AI allocation across enterprise roles, or a tested methodology for making work units AI-digestible. The field has emerging methods, but mature execution remains uncommon — Deloitte's 6% makes that gap visible.

What I have is a design sequence built from first principles across this series, and a conviction: the transformation everyone is looking for will not come from governing AI better alone. It will come from redesigning work to fit what AI actually is — the third participant in the history of the workforce, and the first one that carries duties without accountability.

Much of the prior art governs the technology or calls for redesign at a high level. The work ahead is to make organizational redesign operational.

---

## References

[1] Frederick Winslow Taylor, *The Principles of Scientific Management*, Harper & Brothers, 1911. Scientific management separates managerial planning and work study from worker execution, emphasizing standardized methods, selection, training, supervision, and time study. <https://www.gutenberg.org/cache/epub/6435/pg6435.html>

[2] Peter F. Drucker, *Landmarks of Tomorrow*, Harper & Brothers, 1959 edition. Identifies the rise of knowledge work and the knowledge worker. Some bibliographic records identify an earlier 1957 publication of the work. <https://search.worldcat.org/title/387964?page=citation>

[3] Peter F. Drucker, *The Practice of Management*, Harper & Brothers, 1954; *Management Challenges for the 21st Century*, HarperBusiness, 1999; and *Managing Oneself*, Harvard Business Review, January 2005. These works develop management by objectives and self-control and the later emphasis on knowledge-worker autonomy and self-management. <https://drucker.institute/books-by-peter-drucker/>; <https://hbr.org/2005/01/managing-oneself>

[4] Marta Stelmaszak, Mayur Joshi, and Ioanna Constantiou, *Artificial Intelligence as an Organizing Capability Arising from Human–Algorithm Relations*, Journal of Management Studies 63, no. 2 (2026), 335–365. A peer-reviewed theoretical article describing AI as organizing new structures, roles, expertise, and organizational practices among human and algorithmic actors. <https://onlinelibrary.wiley.com/doi/10.1111/joms.70003>

[5] Emma Wiles, Megan Hsu, Julie Bedard, and Matthew Kropp, *Putting AI on the Org Chart: Evidence on Delegation and Oversight*, working paper, 2026. Combines a survey of 1,261 HR and finance managers, a randomized experiment with 813 analyzed participants, and a separate weighted survey of 1,500 U.S. senior managers. The experiment compares identical work labeled as produced by an AI tool, an AI employee, or a human employee. <https://www.emmawiles.com/storage/ai_employee.pdf>

[6] NIST, *Artificial Intelligence Risk Management Framework (AI RMF 1.0)*, January 2023; ISO, *ISO/IEC 42001:2023 — Information technology — Artificial intelligence — Management system*, December 2023. NIST AI RMF is voluntary, non-sector-specific guidance and was under revision in 2026; ISO/IEC 42001 is a requirements-based management-system standard. <https://www.nist.gov/publications/artificial-intelligence-risk-management-framework-ai-rmf-10>; <https://www.iso.org/standard/42001>

[7] Matthew Kropp, Julie Bedard, Emma Wiles, Megan Hsu, and Lisa Krayer, *Research: Why You Shouldn't Treat AI Agents Like Employees*, Harvard Business Review, 6 May 2026. Reports the same underlying research program as [5], emphasizing that employee framing reduced individual accountability and review quality, increased unnecessary escalation, and heightened role uncertainty without improving adoption. <https://hbr.org/2026/05/research-why-you-shouldnt-treat-ai-agents-like-employees>

[8] Microsoft, *2025 Work Trend Index Annual Report: The Year the Frontier Firm Is Born*, 2025. Twenty-eight percent of managers said their organizations were considering hiring AI workforce managers to lead hybrid teams. The global survey covered 31,000 knowledge workers across 31 markets, with an additional U.S. sample. <https://www.microsoft.com/en-us/worklab/work-trend-index/2025-the-year-the-frontier-firm-is-born>

[9] Mercer, *Global Talent Trends 2026: Solving the Human–Machine Equation*, February 2026. Survey of nearly 12,000 executives, HR leaders, investors, and employees across 16 geographies and 16 industries. Eighty-two percent of C-suite respondents said the future of HR lies in managing human talent and digital agents side by side; 63% ranked redesigning work for AI and automation as their highest-ROI people priority. <https://www.mercer.com/about/newsroom/mercer-s-global-talent-trends-2026-report/>

[10] Deloitte, *2026 Global Human Capital Trends*, 2026. More than 9,000 business and HR leaders across 89 countries were surveyed; 6% said they were making progress designing human–AI interactions. The report also says organizations intentionally redesigning roles, workflows, and decision-making are more likely to exceed expected investment returns. <https://www.deloitte.com/us/en/insights/topics/talent/human-capital-trends.html>; <https://www.deloitte.com/us/en/about/press-room/deloitte-report-winning-organizations-will-build-the-human-advantage.html>

---

## Earlier in this series

- [The Enterprise Chose Speed. Own the Burden.](Post 12)
- [The Models Are Getting Better. Code Still Cannot Fully Check Their Judgment.](Post 11)
- [The CPU Stack Runs on Specification. The LLM Stack Runs on Judgment.](Post 10)
- [AI Takes Duties, Not Accountability](Post 6)
- [When AI Splits the Duties, Redraw the Business Process](Post 5)
- [AI Took Programming, Not Engineering. Will It Do the Same to Other Roles?](Post 4)
- [Does Software Ever Need to Call an Agent?](Post 3)
