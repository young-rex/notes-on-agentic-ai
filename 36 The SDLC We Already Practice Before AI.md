# 36. The SDLC We Already Practice, Before AI

*Originally published on LinkedIn: <https://lnkd.in/p/gtYC_M4g>*

## A small change, and everyone it touches

A retailer changes its refund policy. Today, customers get a full refund within 30 days of delivery. From March 1, they still get a full refund within 30 days, but from day 31 to day 60 they get store credit instead. Defective items can be refunded for 90 days. Final-sale items can't be returned. Orders placed before March 1 keep the old rule. A refund over $500, or a customer's third refund in 12 months, goes to a person for review.

It's a small change. Count the people it passes through. Someone in the business settles the rule and its exceptions. Someone works out which systems and data the rule touches. Someone decides what counts as correct. Developers change the code and the tests. Reviewers and a release process decide that it can ship. Operations watches what happens. Customer service hears what customers think, and that starts the next change.

You may call these activities something else: backlog refinement, a design review, a pull request, a release pipeline, an incident, a maintenance ticket. Whatever the names, this is the software development life cycle (SDLC) as it is practiced. More than a diagram on a wall, it is the path a change takes through an organization.

Post 18 described enterprise systems before AI and set aside the practices used to build and run them: "Those fields are also being changed, and they deserve their own treatment." Post 33 looked inside the development team during one iteration. This post widens the view to the whole path of a change through practices established before AI that remain in use today. The next post follows the same change as agents take on parts of the work. The refund example is familiar from Post 16, where an agent decided refund cases. Here the rule lives in code; the next post returns to the difference.

**The lifecycle is the path a change takes, and each of us works somewhere along it.**

## Following the change

Here is the refund change, duty by duty. The artifacts will look familiar; who holds each duty varies.

- **Settle the rule.** A business owner, product owner, or analyst turns the policy into requirements: user stories, acceptance criteria, a policy document. Questions surface early. Does day 31 count from purchase or from delivery? Who decides that an item is defective? As Post 33 put it, the requirements tell the development team what to build and give the business its criteria for accepting the result.
- **Find what it touches.** The order system, the returns service, payments, a store-credit ledger, the customer-service tools, finance reporting. An architect, developer, or analyst traces the impact through code, data, and documentation that someone else wrote years ago.
- **Decide what counts as correct.** Acceptance cases make the rule testable: a return on day 30 and on day 31, a defective item on day 75, a final-sale item, an order placed the day before March 1, a $501 refund, a customer's third refund in 12 months. A product owner, tester, or business owner agrees on which outcomes are right.
- **Change the code and the tests.** Developers work in branches and pull requests, with continuous integration running the tests on every change.
- **Review and release.** Someone reviews the code. Someone, or some process, approves the release. The change ships behind a feature flag, one region at a time. Turning the flag off stops the new rule for later returns; it doesn't take back store credit already issued.
- **Operate and observe.** Dashboards track the refund rate, store-credit issuance, and support tickets. Alerts and incidents go to whoever runs the service: an operations team, a site reliability team, a platform team, or the developers themselves.
- **Feed back.** Support sees more tickets about day-31 store credit. A backlog item follows: clarify the message at checkout. The next change begins.

Three things about this list matter more than its order. First, it isn't a straight line. A tester's question sends the team back to the rule; an operations finding starts new work. Second, one person may hold several of these duties. In a small company, one developer may settle details with the owner, write the code, release it, and watch it run. In a large one, each duty may belong to a different department. Third, some duties are decisions, and deciding takes authority as well as knowledge. Passing tests provide evidence that the code matches the agreed cases; someone still accepts the result for the business, and someone, or some process, approves the release.

The path also loses information. In NaPiRE, a survey of requirements engineering in 228 companies across 10 countries, respondents chose the five most critical problems from a list of 21. The two named most often were incomplete or hidden requirements (48%) and communication flaws between the project team and the customer (41%) [1]. The survey ran in 2014 and 2015, and its authors can't estimate how representative it is. Still, anyone who has seen an exception surface after release will recognize the pattern.

**The duties persist across organizations; who holds them and how they connect vary.**

## Why colleagues see different parts

People on the refund change can know their own duties deeply and still see different parts of the path. Part of the reason is where they started.

Computing degrees are designed to emphasize different parts of the path. The CC2020 report asked its steering committee to rate how important 34 knowledge areas are for each computing discipline, on a scale from 0 to 5 [2]. Selected ratings, by discipline:

- **Information systems (IS):** systems analysis and design 4–5; software process and programming fundamentals 1–3.
- **Software engineering (SE):** requirements analysis and software process 3–5.
- **Computer science (CS):** programming fundamentals 4–5; systems analysis and design and requirements analysis 1–2; software process 1–3.
- **Information technology (IT):** "virtual systems and services" and "integrated systems technology" 3–4; software process 1–3.

These are expert opinions, not a survey of graduates, and they describe what programs intend to emphasize.

The curricula show the same pattern. IS2020 frames systems analysis around "the correct definition of business problems and user requirements." One of its competencies is "Demonstrate the SDLC phases and activities"; another is "Describe the role and responsibilities of the participants in the SDLC" [3]. CS2023 moved its software engineering material toward "the software workflow (version control, testing, code review, tooling)," with "less focus on team leadership and project management," aiming at material "relevant immediately upon graduation" [4]. SE2014 covers the process itself, including release management, continuous integration, maintenance, and legacy systems [5]. None of this tells us what any graduate knows. Programs add their own courses, and a degree is only a beginning.

Work is another starting place. Craig and colleagues interviewed 20 early-career developers with four-year CS degrees and concluded that "many new software developers feel ill-prepared in multiple areas including tool use, communications, teamwork and working on large, long-lasting, open-scoped, complex software systems" [6]. The time these developers spent on legacy code ranged from none to 95%. The study is small and covers early careers. It shows challenges that some new developers met at work; it doesn't measure how much of anyone's lifecycle knowledge comes from the job.

Roles and structures shape the view too. A tester sees acceptance; an operations engineer sees incidents; an analyst sees the rule and its exceptions. Who sees production depends on how the organization is built. In 37 interviews, Leite and colleagues found four arrangements of development and infrastructure work: siloed departments, classical DevOps, cross-functional teams, and platform teams [7]. In a siloed arrangement, the developer who changed the refund code may never see the support tickets.

Knowing the formal names is a separate matter again. In a survey of 90 requirements practitioners, about 47% of the requirements engineers and business analysts did not know ISO/IEC/IEEE 29148, the core requirements engineering standard [8]. Someone can do sophisticated work without knowing the reference that describes it, and naming the reference doesn't demonstrate the work.

**Where colleagues started and where they work shape which parts they see.**

## Responsibilities, methods, and tools

Discussions of the SDLC and Agile can mix three different layers.

- **Responsibilities** are the duties the refund change needed: settle the rule, analyze the impact, decide what counts as correct, change the code, release, operate, and feed back. They exist whatever the method.
- **Methods** organize those duties: when they happen, in what size of batch, with which meetings and artifacts. Scrum, Kanban, plan-driven development, and DevOps practices are methods.
- **Tools** support or carry out parts of the arrangement: Jira, Git, a CI server, a deployment pipeline.

In Scrum, the refund change might become a few backlog items delivered over one or two sprints. In a plan-driven project, it might be a work package with a scheduled release. The duties are the same; the method decides their timing and batch size.

SE2014 states the distinction directly. The engineering practices it describes "may be mapped into a variety of different software development processes (such as plan-driven, iterative, and agile)" [5]. The Scrum Guide says the same thing from the method's side. Scrum is "a lightweight framework that helps people, teams and organizations generate value through adaptive solutions for complex problems," and "the Scrum framework is purposefully incomplete, only defining the parts required to implement Scrum theory." In its words, "various processes, techniques and methods can be employed within the framework" [9]. Scrum doesn't say how to test the day-31 boundary or roll back a release. The team supplies those practices.

Agile didn't replace the lifecycle. Each of the Agile Manifesto's four values weighs one thing against another, such as "Responding to change over following a plan," and the manifesto adds: "That is, while there is value in the items on the right, we value the items on the left more" [10]. SE2014 puts it this way: "agile thinking does not discard earlier ideas and concepts; it adjusts the emphasis given to different aspects of the development process" [5].

Why, then, do people speak of the SDLC versus Agile, or of an SDLC that merged with Agile? My reading is that part of the answer is vocabulary. IS2020 lists Agile, Waterfall, V-shaped, iterative, and spiral as "SDLC Models" [3]. SE2014 treats them as characteristics of life-cycle models within the software process and maps the same practices onto each [5]. Read one way, Agile is a kind of SDLC. Read the other way, it is a way of organizing the SDLC's work. This post uses the second reading: the responsibilities stay, and methods arrange them.

Two surveys show how methods were combined in the projects they sampled. The HELENA survey collected 1,467 responses in 2017. Among the 556 analyzed in one study, 83 (about 15%) reported a consistent agile, traditional, or balanced approach across their project disciplines. The authors conclude: "With some 15% of the studied population, the 'pure doctrine' accounts for a small share only" [11]. In NaPiRE, 58 of the 196 respondents who named their process model called it mixed [1]. Both are convenience samples, so neither gives an industry-wide percentage. Within both samples, combined approaches were common.

Tools sit a layer below. A team can run Scrum in Jira, in another tool, or on a whiteboard. Using the tool doesn't show that the method is followed. The next post needs this distinction, because AI arrives both as methods and as tools, sometimes under the same name.

**Methods organize the responsibilities and tools support the methods; neither removes the responsibilities.**

## Release, operation, and the next change

The refund change isn't finished when the code merges. Over recent decades, continuous delivery and DevOps have pulled release and operation closer to development. The feature flag and the regional rollout belong to those practices.

Who approves the release varies, and the choice matters. DORA (DevOps Research and Assessment) reports that heavyweight external approvals "have a negative impact on software delivery performance," and that it found no evidence that formal external review reduces change failures. It recommends peer review recorded in the development platform, with automated testing and monitoring to catch bad changes. Change advisory boards keep a role in coordinating between teams and making business trade-offs [12]. These findings come from surveys and show associations, not controlled results. They do show that approval can be arranged in more than one way.

Who runs the software varies too. In 2006, Amazon's CTO, Werner Vogels, described the alternative to a hand-off: "The traditional model is that you take your software to the wall that separates development and operations, and throw it over and then forget about it. Not at Amazon. You build it, you run it" [13]. That is one company's practice; Leite's four arrangements show the wider range [7].

Two of the field's cornerstone references also place this work within the lifecycle. The SWEBOK Guide, the IEEE Computer Society's body of knowledge for software engineering, made Software Engineering Operations a separate knowledge area in version 4; the current release is V4.0a [14]. ISO/IEC/IEEE 12207, the international standard for software life cycle processes, was revised in 2026; it establishes "a common process framework for describing the full life cycle" of software, from conception through retirement [15]. Operation, and eventually retirement, are part of the lifecycle, not what comes after it. One day the refund service itself will be replaced, and that will be lifecycle work too.

Then the support tickets arrive, and the next change starts. Post 30 named operating outcomes as a responsibility that stays on our side: the model cannot answer for results, and "when the software runs, someone on our side must."

**Release and operation are lifecycle work, and they start the next change.**

## Described, implemented, shown to work

Each arrangement in this post, from a team's method to its release approval, comes with claims about how the work is done and how well it works. A method's name and a team's actual arrangement are different kinds of evidence. "We do Scrum" names a framework, and by its own guide that framework leaves room for other practices. The name alone doesn't show which practices the team uses. The Agile Manifesto states values; it isn't a measurement of how organizations adopted them.

Three questions keep these apart:

- **Is it described?** A curriculum, a guide, or a manifesto says what should happen.
- **Is it implemented?** A team's tools, pipelines, and records show what is actually set up.
- **Has it been shown to work, where, and for what?** A study or report measures results for a particular sample, setting, and outcome.

This post's sources speak at different levels. The curricula describe intent. The Scrum Guide describes a framework. DORA reports associations measured in surveys. Vogels reports one company's practice. A single source can answer more than one question, but evidence that answers one doesn't automatically answer the others.

The same questions apply to every AI-native proposal in the next post. A playbook may describe an arrangement, a tool may implement part of it, and a study may measure one outcome. Each claim has to be read at its actual scope.

**Ask all three of any method: is it described, is it implemented, and has it been shown to work, where, and for what?**

## The work we divide and coordinate

The SDLC is lived engineering practice. Its responsibilities persist, methods organize them, and tools support the methods. Education, role, experience, and how an organization is built all affect which parts colleagues see. The lifecycle works as a whole even though its duties are split across people.

What holds it together is connection, not universal knowledge. Meetings, tickets, reviews, approvals, dashboards, and incident reports move information and authority between people who each see part of the path. Knowing the refund policy, seeing production data, and being authorized to release are separate conditions, held by different people in different combinations.

Post 30 put the agent's position simply: "The agent knows only what we give it." That sets the question: what changes when a participant can perform substantial duties but sees and acts only through the context, tools, and permissions it is given?

**The lifecycle is work we already divide and coordinate. The next post asks what changes when an agent takes on part of it.**

---

## References

[1] Daniel Méndez Fernández et al., *Naming the Pain in Requirements Engineering: Contemporary Problems, Causes, and Effects in Practice*, Empirical Software Engineering, 2017. <https://arxiv.org/abs/1611.10288>. A survey of 228 companies in 10 countries, run in 2014–15; respondents chose their five most critical problems from 21. The authors can't estimate how representative the sample is.

[2] CC2020 Task Force, *Computing Curricula 2020: Paradigms for Global Computing Education*, ACM and IEEE Computer Society, December 31, 2020. <https://www.acm.org/binaries/content/assets/education/curricula-recommendations/cc2020.pdf>. Table 5.3. The ratings are rounded means of steering-committee members' expert opinions, "instead of, for example, a representative survey of the disciplines."

[3] Joint ACM/AIS IS2020 Task Force, *IS2020: A Competency Model for Undergraduate Programs in Information Systems*, ACM and AIS, December 2020. <https://www.acm.org/binaries/content/assets/education/curricula-recommendations/is2020.pdf>. The Systems Analysis and Design competency area, Appendix A3.4.1.

[4] Joint Task Force on Computer Science Curricula, *Computer Science Curricula 2023*, ACM, IEEE Computer Society, and AAAI, January 2024. <https://ieeecs-media.computer.org/media/education/reports/CS2023.pdf>. The Software Engineering knowledge area, "Changes since CS 2013."

[5] Joint Task Force on Computing Curricula, *Software Engineering 2014: Curriculum Guidelines for Undergraduate Degree Programs in Software Engineering*, IEEE Computer Society and ACM, February 23, 2015. <https://ieeecs-media.computer.org/assets/pdf/se2014.pdf>. Pages 13, 26, and 35.

[6] Michelle Craig, Phill Conrad, Dylan Lynch, Natasha Lee, and Laura Anthony, *Listening to Early Career Software Developers*, CCSC Southwestern Conference, Consortium for Computing Sciences in Colleges, 2018. <https://sites.cs.ucsb.edu/~pconrad/files/paper028.pdf>. Interviews with 20 developers holding four-year CS degrees and one to five years' experience; a convenience sample from four schools.

[7] Leonardo Leite, Gustavo Pinto, Fabio Kon, and Paulo Meirelles, *The Organization of Software Teams in the Quest for Continuous Delivery: A Grounded Theory Approach*, Information and Software Technology, 2021. <https://arxiv.org/abs/2008.08652>. 37 semi-structured interviews with IT professionals.

[8] Xavier Franch, Martin Glinz, Daniel Mendez, and Norbert Seyff, *A Study about the Knowledge and Use of Requirements Engineering Standards in Industry*, IEEE Transactions on Software Engineering, 2022. <https://arxiv.org/abs/2105.13961>. A survey of 90 requirements practitioners.

[9] Ken Schwaber and Jeff Sutherland, *The Scrum Guide*, November 2020. <https://scrumguides.org/scrum-guide.html>

[10] Kent Beck et al., *Manifesto for Agile Software Development*, 2001. <https://agilemanifesto.org/>

[11] Marco Kuhrmann, Paolo Tell, Regina Hebig, Jil Klünder, et al., *What Makes Agile Software Development Agile?*, IEEE Transactions on Software Engineering, 2021. <https://arxiv.org/abs/2109.11435>. Data from the HELENA survey: 1,467 responses collected May–November 2017 by convenience sampling, of which 556 were analyzed. The authors don't claim generalizability.

[12] DORA, *Capabilities: Streamlining change approval*, accessed October 2026. <https://dora.dev/capabilities/streamlining-change-approval/>. DORA's findings come from surveys; they are associations, not controlled results.

[13] *A Conversation with Werner Vogels: Learning from the Amazon Technology Platform*, interview by Jim Gray, ACM Queue 4(4), 2006. <https://doi.org/10.1145/1142055.1142065>. One company's practice, described by its CTO.

[14] Hironori Washizaki (ed.), *Guide to the Software Engineering Body of Knowledge (SWEBOK Guide), Version 4.0a*, IEEE Computer Society, released August 2026. <https://ieeecs-media.computer.org/media/education/swebok/swebok-v4.pdf>. Software Engineering Operations is Chapter 6.

[15] *ISO/IEC/IEEE 12207:2026, Systems and software engineering — Software life cycle processes*, published April 15, 2026, superseding the 2017 edition. <https://standards.ieee.org/ieee/12207/11416/>. The scope is quoted from IEEE's listing; the standard's full text is paywalled.

## Earlier in this series

All posts are archived in the GitHub repository: <https://github.com/young-rex/notes-on-agentic-ai>

- Post 33: *Developing C1 Software with AI: Today and the Path Ahead* <https://lnkd.in/p/gE9xvGSd>
- Post 30: *AI Coding Has Two Sides. First, Theirs.* <https://lnkd.in/p/gyg_buW9>
- Post 18: *Enterprise Systems Before AI: Academia, Vendors, Architecture, Process, Data, and Operations* <https://lnkd.in/p/gPgiypEM>
- Post 16: *Calibrate: Choose a Model for Enterprise Refund Decisions* <https://lnkd.in/p/gk_7yGX5>
