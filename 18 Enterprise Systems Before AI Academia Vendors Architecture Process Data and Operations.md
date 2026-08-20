# Enterprise Systems Before AI: Academia, Vendors, Architecture, Process, Data, and Operations

The previous post used the ENIAC/LEO history to make a simple point: AI, like computing before it, is being pulled quickly into business use.

But AI enters a very different enterprise from the one LEO entered. It arrives after seventy-five years of enterprise computing, into a landscape already filled with systems, categories, owners, and seams.

That landscape was never one thing with one classification. It was classified several times over, by different communities who did not agree with each other and had no particular reason to. MIS textbooks, ERP vendors, enterprise architects, process modelers, data warehouse practitioners, and industrial control engineers each classified the same enterprise in a different way. Each was useful and authoritative inside its own area. Each was built to answer a question the others were not asking.

This article puts those existing classifications side by side, so we can see both where AI arrives and what it might actually change. It walks through six of them — academic information systems, vendor suites, CIO architecture, process, data, and operations — with particular attention to where they disagree. Those disagreements are not failures of coordination waiting to be resolved; they are permanent features of the enterprise, and they are what AI is now reaching into.

## What this article means by "enterprise systems"

A narrow sense: the runtime business systems that record, coordinate, analyze, and control enterprise work. ERP, CRM, SCM, HCM, BPM and workflow platforms, BI and data warehouses, MES, SCADA, and their relatives.

It does not cover the systems and practices used to design, build, deploy, and maintain those systems — software engineering, DevOps, CI/CD, code review, project management, IT service management, architecture governance as a practice, security engineering, software delivery. Those fields are also being changed, and they deserve their own treatment. They are a different landscape.

With that boundary set: before AI transformation, how was this landscape already classified?

## The short answer: by whoever had to manage it

An academic classifying systems to teach them, a vendor classifying systems to sell them, and a CIO classifying systems to fund them are not doing the same job, and there is no reason their categories should line up.

The six views below are an editorial selection, not a claim that enterprise systems have exactly six classifications. I chose them because each comes from a community with durable authority over runtime business systems: how they are taught, bought, funded, designed as work, reported on, or operated. Other views are possible. The point here is to establish the classifications this series needs before turning to what AI changes.

| The authority | Classifies by | Categories it produced |
|---|---|---|
| Academic information systems | Management level and decision structure | TPS, MIS, DSS, EIS/ESS |
| Enterprise suite vendors | Purchasable business function | ERP, CRM, HCM, SCM, EPM, ECM, PLM, EAM |
| Enterprise architects and CIO advisors | Portfolio role and pace of change | Gartner: record, differentiation, innovation. Forrester: engagement, insight |
| The business process community | The shape of the work | Workflow, BPM, case management, decision management |
| The data and analytics community | Workload — recording versus analyzing | OLTP, OLAP, warehouse, mart, ODS, BI |
| Operations and industrial systems | Control boundary and time scale | ISA-95 Levels 0 to 4: process, sensors, PLCs, MES/SCADA, ERP |

Six views. Same enterprise. Take them one at a time.

## The academic view: systems by management level

The oldest classification came from business schools, and it was not originally about computers at all.

In 1965, Robert Anthony split organizational planning and control into three levels: strategic planning, management control, and operational control [1]. Different levels of an organization face different questions, on different time horizons, with different tolerance for ambiguity. The board deciding which markets to enter and the clerk deciding whether to accept a return are both making decisions, and almost nothing else about the two is alike.

In 1960, Herbert Simon distinguished programmed decisions from non-programmed decisions [2]. In 1971, Gorry and Scott Morton combined Simon's decision-structure distinction with Anthony's management levels, using the structured, semi-structured, and unstructured vocabulary that stuck [3]. Some decisions can be fully specified — the inputs are known, the rule is known, the answer follows. Some are semi-structured. Some are unstructured, where even the shape of the problem is a matter of judgment.

Cross management level with decision structure and you get a grid, and the grid tells you what kind of system to build. Structured decisions at the operational level could be automated outright. Unstructured decisions at the strategic level could only be supported — give the executive better information and let them decide.

From that grid came the vocabulary generations of students learned [4]:

- **Transaction Processing Systems** — record the operational events of the business
- **Management Information Systems** — summarize those events into periodic reports for middle management
- **Decision Support Systems** — model and analyze semi-structured problems interactively
- **Executive Information or Support Systems** — surface highly aggregated signals for senior leadership

Some treatments add office automation and knowledge work systems alongside these, sitting at the level between operations and management.

These categories are now taught mainly within the discipline of information systems.

**What this view shows:** organizational level, information need, and how structured the underlying decision is.

**Where it had power:** in the curriculum. This classification trained the people who went on to run enterprise IT, and it gave them a shared vocabulary for talking about what a system is *for*.

**The seam it leaves:** the categories outlived the products that carried them. Decision support did have a software market of its own in the 1980s [5], and then the market moved on — much the same capability was sold again as business intelligence, and again as analytics, under names the textbook never adopted. The academic classification named a function and held it steady for decades. The market named products and renamed them every decade or so. Neither was wrong, and they never lined up. It is a specific kind of authority: enough to shape how a generation *talks* about systems, not enough to shape what they *buy*.

## The vendor view: systems as business suites

The classification with the most practical force came from the companies selling the software.

SAP, Oracle, Microsoft, Salesforce, Workday, IBM, and their peers classify enterprise systems by business function, packaged as something you can buy: ERP, CRM and CX, HCM, SCM, EPM, procurement, ECM, PLM, EAM, warehouse management, and an expanding set of industry-specific applications [6][7][8].

That classification has power because it is the shape the enterprise actually acquires. Budget lines follow module names. So do project names, team names, org units, and job titles. "SAP FI" is simultaneously a software module, a department, a set of consultants, and a career. Once an enterprise has bought in that shape, it thinks in that shape.

The important thing about a packaged module is that it is not a neutral container. It arrives with assumptions about how the work should be done. Thomas Davenport made this argument in 1998, when ERP was becoming standard: an enterprise system imposes its own logic on a company's strategy, organization, and culture, and the choice to adopt one is a business decision rather than a technical one [9]. You can bend the software to fit your process, or bend your process to fit the software. Both cost money. Choosing which to bend is what an implementation program largely *is*.

**What this view shows:** business function as a purchasable, configurable, implementable unit.

**Where it had power:** the market. The vendor view wins ties, because it is the only one that comes with a price and a delivery date.

**The seam it leaves:** the module boundary is not the process boundary. Order-to-cash does not live in one module — it crosses sales, credit, inventory, fulfillment, and finance, and touches the customer more than once. The vendor draws a box around a function; the actual work runs straight through several boxes and out the other side. Which is exactly what the next two views are about.

## The CIO view: systems by portfolio role

Once an enterprise owns four hundred applications, a different question takes over. Not *what does this system do* — you cannot hold four hundred answers to that — but *how should I treat it*. Fund it or freeze it. Standardize it or let it differentiate. Replace it or wrap it. Govern it tightly or let the team move.

Enterprise architects and CIO advisory firms built a classification for that question, and its categories describe portfolio role rather than function. This is also the one view whose vocabulary came from more than one source, so the sources are worth keeping apart.

The shared starting point is the **system of record** — the authoritative source of business truth. Slow-moving, heavily governed, expensive to break. The term is older than either framework below, and neither owns it. Each builds outward from it in a different direction.

Gartner's pace-layered application strategy sorts the portfolio by how fast each part is expected to change and how much of the business's distinctiveness it carries [10]:

- **Systems of record** — the common, standardized core, changed slowly and deliberately.
- **Systems of differentiation** — capabilities specific to how this company competes, changed on a business cycle.
- **Systems of innovation** — new and experimental, expected to change constantly, and expected to be discarded sometimes.

Geoffrey Moore introduced systems of engagement in a 2011 AIIM white paper, contrasting them with systems of record; Forrester later adopted the term [11][12]. Forrester's systems of insight extended the portfolio vocabulary in another direction [13]:

- **Systems of engagement** — the fast-changing layer where people actually interact with the business, defined explicitly against the slower record layer.
- **Systems of insight** — where data is turned into analysis and fed back into decisions.

Two frameworks, overlapping but not identical, both in daily use and frequently in the same sentence. Even inside the CIO/advisory view, there was no single owner of the vocabulary.

**What this view shows:** rate of change, governance intensity, business distinctiveness, and where the truth lives.

**Where it had power:** the budget. This is the view that decides what gets modernized, what gets outsourced, what gets an integration layer, and what gets left alone for another five years.

**The seam it leaves:** the record and engagement layers are describing the same data with incompatible demands. The customer record must be authoritative, consistent, and changed carefully. The customer-facing application must answer in milliseconds and ship a new version every two weeks. Both requirements are legitimate. Neither yields. Essentially the entire middle of enterprise architecture — APIs, caches, event streams, integration platforms, read replicas — exists to hold those two expectations apart so that neither has to compromise.

## The process view: systems by the shape of the work

The process community looks at the enterprise and does not see applications at all. It sees work moving: who does what, in what order, with what handoffs, and what happens when the normal path does not apply.

Its categories describe kinds of work and the platforms that support them — workflow systems, business process management suites, and the familiar split between human-centric, integration-centric, and document-centric process automation [14].

The clearest expression of this view is its three modeling standards, which the Object Management Group publishes as a family [15][16]:

- **BPMN** models a process — a flow that can be drawn in advance, with tasks, gateways, roles, and events.
- **CMMN** models a case — work whose path depends on what is discovered along the way.
- **DMN** models a decision — the rules and logic pulled out of the flow so they can be stated, versioned, and changed on their own.

The existence of the second one is the interesting part. Case management exists because the process community ran into work it could not fully specify in advance [17]. An investigation, an insurance claim, a patient's course of treatment, a complex complaint — these have a goal and a set of available activities, but the order depends on what turns up. You cannot draw the flow, because the flow is decided as the work proceeds.

So the community drew what it could, and built a second notation for the rest.

**What this view shows:** handoffs, reviews, exceptions, escalations, and the human judgment sitting inside them. An application view shows boxes; the process view shows the gaps between the boxes and who carries the work across.

**Where it had power:** in operations and in audit. Processes are what regulators inspect, what controls attach to, and what gets redesigned when the business changes.

**The seam it leaves:** two of them, actually. The process runs across the vendor's modules, so the process view and the application view are permanently misaligned. And the enterprise has always had work that resists specification — handled not with better flow diagrams but with cases, exception queues, and someone with the authority to decide. That was true long before anything new arrived.

## The data view: systems by workload

The data community classifies systems along a single decisive line: recording that something happened and analyzing what happened are different jobs, and the same system should not try to do both.

Recording is many small writes, about current state, that must be fast and exactly right. Analysis is a few large reads, across history, that aggregate. Push both through one database and each degrades the other — the reporting query locks the table the order entry needs. So the landscape split in two [18][19]:

- **OLTP** — operational systems, current state, transaction workload
- **Operational data store** — lightly integrated operational data, near-current
- **Data warehouse** — integrated, historical, subject-oriented, built for query
- **Data marts** — warehouse subsets shaped for a particular business area
- **BI, reporting, dashboards** — the consumption layer where people actually look

This community also disagreed with itself, productively and for decades. Bill Inmon argued for a top-down enterprise-wide warehouse from which marts are derived [20]; Ralph Kimball argued for bottom-up dimensional marts integrated through conformed dimensions [21]. Two methodologies, both mainstream, both defensible. An authority with an internal argument is still an authority — it just means the practitioner has to pick a side.

**What this view shows:** workload shape, latency, history, and semantic consistency across sources.

**Where it had power:** in the reporting line. This view decides what the numbers *are* — which is to say, what the business believes about itself.

**The seam it leaves:** two answers to one question. Ask "how much inventory do we have?" and the operational system and the warehouse will both answer, and the answers may differ, because one is live and the other reflects last night's load with different business rules applied. Neither is wrong inside its own view. Reconciliation — a permanent, staffed, recurring activity in most large enterprises — exists because the views disagree and somebody has to explain the gap every month.

## The operations view: systems by control layer and time scale

The last view comes from industrial operations, and it is the only one where speed is stated openly. It belongs to this tour because the principle it makes explicit — that systems closer to physical consequence must answer faster — becomes most visible where enterprise systems touch physical operations.

ISA-95, published internationally as IEC 62264, defines the integration between enterprise systems and control systems by arranging them in levels [22][23]:

| Level | What runs there | Roughly how fast |
|---|---|---|
| 4 | Business planning and logistics — ERP | days to months |
| 3 | Manufacturing operations management — MES/MOM, SCADA, historians | seconds to shifts |
| 2 | Monitoring and supervising control — PLCs, DCS, control systems | sub-seconds to seconds |
| 1 | Sensing and manipulating the process — sensors, actuators, instruments | milliseconds to seconds |
| 0 | The physical process itself | continuous |

The time scales are approximate, and the standard's real subject is the activities at each level and the information that crosses between them [24]. Product names can vary at the boundaries; the table follows the ISA and OPC activity placement, which puts PLCs and DCSs at Level 2 and SCADA among systems supporting Level 3 activities. But the ordering principle is unmistakable: a level is defined by how fast it must respond and how close it sits to physical consequence. A pressure loop cannot wait for a planning cycle. A planning cycle cannot be run at millisecond cadence.

ISA-95 is a manufacturing standard and should be read as one. The same need for layered control appears in other physical-operation domains — utilities and grid control, logistics and warehouse floors, telecommunications network operations — though not always through ISA-95 itself, and often with standards and vocabulary of their own.

**What this view shows:** control boundaries, native time scales, and the OT/IT divide.

**Where it had power:** in plants and infrastructure, where the consequence of being wrong is physical and immediate.

**The seam it leaves:** ERP and MES do not agree on when work is finished. To Level 4, a work order is complete when the transaction posts. To Level 3, it is complete when the physical process actually finishes. Those can be hours apart, and during those hours the two systems describe the world differently. ISA-95 exists precisely because the levels disagreed and somebody had to negotiate the boundary. That is worth remembering: a standard is what a tension looks like after someone has resolved it in writing.

## No one owned the whole picture

Put the six side by side and a single system fractures into six descriptions. Take the ERP at a manufacturer — one deployment, one database, one set of screens:

| The same ERP, seen by | Is a |
|---|---|
| An academic | Transaction processing system |
| A vendor | ERP, sold in modules |
| An enterprise architect | System of record, slow layer, tightly governed |
| A process architect | The execution point of order-to-cash |
| A data architect | The OLTP source feeding the warehouse |
| An operations architect | Level 4, above MES |

None of these is wrong. Each is correct inside its own area, and each was built to answer a question the others were not asking.

But they are not six layers of a single truth, either. They contradict, in specific and recurring places:

- a category the curriculum kept, and the market renamed
- a module boundary that the actual process ignores
- one customer record, owed both stability and speed
- a flow you can draw, and work that will not hold still to be drawn
- one inventory question with two defensible answers
- one work order, complete at two different times

Each of those seams has work attached to it. Integration platforms exist there. So do reconciliation processes, master data management programs, exception queues, escalation paths, standards committees, and a good share of what an ERP implementation costs. None of that work appears in any single classification — it is not a system, not a module, not a level. It exists because no one classification was sufficient, and something had to hold the gaps.

That is the landscape as it stood before AI arrived: classified repeatedly, by people with different reasons for doing so, with work of its own accumulating at the joins.

Anything new arrives into that. What happens when it does is the subject of the next article.

---

## Sources

[1] Robert N. Anthony, *Planning and Control Systems: A Framework for Analysis*, Harvard University Graduate School of Business Administration, 1965. Separates strategic planning, management control, and operational control. <https://books.google.com/books/about/Planning_and_Control_Systems.html?id=4EeyAAAAIAAJ>

[2] Herbert A. Simon, *The New Science of Management Decision*, Harper & Brothers, 1960. Distinguishes programmed and non-programmed decisions. <https://openlibrary.org/works/OL1205032W/The_new_science_of_management_decision>

[3] G. Anthony Gorry and Michael S. Scott Morton, "A Framework for Management Information Systems," *Sloan Management Review* 13, no. 1, Fall 1971, 55-70. Combines Anthony's management levels with Simon's decision-structure distinction. <https://cir.nii.ac.jp/crid/1573950398810762624>

[4] OpenStax, *Foundations of Information Systems*, "Introduction to Information Systems." Summarizes the TPS, MIS, DSS, and EIS/ESS categories. <https://openstax.org/books/foundations-information-systems/pages/1-1-introduction-to-information-systems>

[5] Daniel J. Power, *A Brief History of Decision Support Systems*, DSSResources.COM, version 4.1; version 4.0 dated March 10, 2007. Documents DSS evolution from the 1960s through the 1980s software market and its transition into executive information systems, OLAP, and business intelligence. <https://dssresources.com/history/dsshistory.html>

[6] Oracle, Fusion Cloud Applications documentation. Vendor suite categories: ERP, SCM, HCM, CX, industry applications. <https://docs.oracle.com/en/cloud/saas/index.html>

[7] IBM, *What are enterprise applications?* Industry category examples including ERP, CRM, SCM, HRM, BI, ECM, and workflow automation. <https://www.ibm.com/think/topics/enterprise-applications>

[8] TechTarget, *ERP modules guide*. Module vocabulary: financials, inventory, order management, WMS, HCM, SCM, CRM, PLM, EAM. <https://www.techtarget.com/searcherp/tutorial/Enterprise-resource-planning-ERP-modules-guide>

[9] Thomas H. Davenport, *Putting the Enterprise into the Enterprise System*, Harvard Business Review, July-August 1998. Argues that enterprise systems impose their own logic on a company's strategy, organization, and culture. <https://hbr.org/1998/07/putting-the-enterprise-into-the-enterprise-system>

[10] Gartner, *Pace-Layered Application Strategy*. Classifies applications as systems of record, differentiation, and innovation by pace of change and business uniqueness. <https://www.gartner.com/en/documents/6504771>

[11] Geoffrey Moore, *Systems of Engagement and the Future of Enterprise IT*, AIIM, 2011. Introduces systems of engagement in contrast with systems of record. <https://info.aiim.org/systems-of-engagement-and-the-future-of-enterprise-it>

[12] Forrester, *A Billion Smartphones Require New Systems Of Engagement*, 2012. Uses Moore's term in contrast with systems of record. <https://www.forrester.com/blogs/12-02-14-a_billion_smartphones_require_new_systems_of_engagement/>

[13] Forrester, *Systems of Insight: Next Generation Business Intelligence*, 2015. <https://www.forrester.com/blogs/15-07-21-systems_of_insight_next_generation_business_intelligence/>

[14] BPMInstitute, *Pick the Right Type of BPMS Solution*. Human-centric, integration-centric, and document-centric BPM suites. <https://www.bpminstitute.org/resources/articles/pick-right-type-bpms-solution/>

[15] Object Management Group, *Business Process Model and Notation*. <https://www.omg.org/bpmn/>

[16] Object Management Group, *BPM+*. Positions BPMN, CMMN, and DMN together as process, case, and decision. <https://www.omg.org/bpmplus/>

[17] IBM, *Business process management and case management*; *Case management concepts*. Distinguishes ordered business processes from case-management situations. <https://www.ibm.com/docs/en/bpm/8.5.6?topic=applications-business-process-management-case-management> · <https://www.ibm.com/docs/en/bpm/8.5.6?topic=overview-case-management-concepts>

[18] Oracle, *Data Warehousing and Business Intelligence*. States that data warehouses are designed for query and analysis rather than transaction processing. <https://docs.oracle.com/cd/B28359_01/server.111/b28318/bus_intl.htm>

[19] IBM, *What is a data warehouse?* OLTP versus OLAP and the BI/warehouse architecture. <https://www.ibm.com/think/topics/data-warehouse>

[20] W. H. Inmon, *Building the Data Warehouse*, first published in 1990 by QED Technical Publishing Group; later editions followed. Defines the data warehouse as subject-oriented, non-volatile, integrated, and time-variant, and advocates a top-down enterprise-wide design. <https://books.google.com/books/about/Building_the_Data_Warehouse.html?id=duRQAAAAMAAJ>

[21] Kimball Group, *The Kimball DW/BI Lifecycle Method*. Represents the business-dimensional DW/BI methodology tradition. <https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dw-bi-lifecycle-method/>

[22] ISA, *ISA-95 Standard: Enterprise-Control System Integration*. <https://www.isa.org/standards-and-publications/isa-standards/isa-95-standard>

[23] Siemens, *ISA-95 framework layers*. Industry explanation of Level 0 physical process through Level 4 business activities. <https://www.siemens.com/en-gb/technology/isa-95-framework-layers/>

[24] OPC Foundation, ISA-95 companion specification. Level definitions with system examples and associated time frames. <https://reference.opcfoundation.org/specs/OPC-10030/4.2.3>
