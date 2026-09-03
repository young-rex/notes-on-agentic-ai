# IT Hiring Is Broken Because Process Outranks Judgment

*Originally published on LinkedIn: <https://lnkd.in/p/gz6Khivv>*

Five posts crossed my feed in about a week. All from different people. All about the same thing.

| Who | What they said | Likes &#124; Comments |
|---|---|---:|
| David Silverander<br>Interviewer | Interviewed 16 people in two weeks. Enraged — not at candidates, but at what they described: AI screens that miss talent, unpaid projects, long loops for jobs that may not exist. | 2,239 &#124; 323 |
| John Vajda<br>20 yrs in tech | Hiring is nearly impossible — not because great people are missing, but because the process itself has broken down. Resumes vanish into systems and never surface. | 312 &#124; 79 |
| Benjamin Neigebauer<br>Sr Principal Engineer | Choked during a live system-design interview. Years of real architecture experience — but no context, no requirements, just "design something on the fly." Asked hiring managers: is this really how you find an architect? | 479 &#124; 182 |
| Michael Erhard<br>COO | Hired 7 people in 8 months by dropping the filters others hide behind: no degree, career gaps, older workers. He could evaluate directly — so he did not need the filters. | 967 &#124; 237 |
| Tim Neumark<br>.NET developer, decades of experience | 400 applications. Zero responses. "Something is wrong." | 558 &#124; 234 |

Roughly 4,500 likes and 1,000 comments across five posts. The frustration is not manufactured.

- David Silverander: https://lnkd.in/p/gQn44Cts
- John Vajda: https://lnkd.in/p/gvnAREnX
- Benjamin Neigebauer: https://lnkd.in/p/gHyd8n8G
- Michael Erhard: https://lnkd.in/p/gEHqjQQG
- Tim Neumark: https://lnkd.in/p/gRjmY5Nk

## Not all of it is a hiring problem

Some of the silence is not a process failure at all. Ghost jobs — listings posted with no current intent to hire — are a documented labor-market distortion [1]. That is a real problem, and it accounts for part of the frustration.

This post is not about that part.

It is about the cases where the company genuinely needs someone, the candidate is genuinely capable, and the process still fails. That is the harder question — and the more useful one, because fraud is a policing problem, but good-faith failure is a design problem.

## This does not make sense. We know what works.

The research on hiring is not ambiguous.

Modern meta-analyses still find structured interviews among the more valid methods for predicting job performance [2]. Reviews of the research explain why: job-related questions, consistent administration, anchored scoring, and disciplined evaluation reduce noise and improve validity [3].

That is the process side. The judgment side is equally clear.

Hartwell and Campion studied more than 20,000 interviews conducted by over 100 interviewers across more than four years — all using the same structured process [4]. Interviewers showed significant leniency and severity differences. Some rated consistently high, others consistently low. In selection simulations, those differences were large enough to change which applicants would have been hired. Normative feedback helped, but its effect decayed over time. Same structure. Same questions. Same anchored rubric. Different interviewers, different ratings — and, in simulations, different selections.

From the other direction, a study followed 7,650 people hired by a major Chinese technology company. Interview notes that mentioned more job-related capabilities were associated with better later job performance and promotion, and with lower turnover [5]. The study did not compare interviewers operating under one strict rubric; interviewers chose their own questions and evaluation approaches. But it points in the same direction: an interview becomes more useful when the evaluator surfaces evidence tied to the actual work.

Weekley et al. found that job-analysis ratings are not mechanical facts — they are judgments made by subject-matter experts, and the most accurate ratings came from people who reported knowing the job extremely well [6]. A separate study found that decision-makers placed more weight on person–job fit when evaluating knowledge-intensive positions [7]. It did not study IT specifically, but the connection matters: in IT roles, fit depends on the stack, the architecture, the domain, the constraints, and the failure modes. Two people can hold the same title and be doing entirely different jobs.

No single study says this outright. Read together, they point one way. Structure disciplines judgment — and the order matters. Judgment is the capability. Structure is what holds it steady. A structured process can make competent interviewers more consistent, but it cannot make weak interviewers competent.

We know what works. Structure and judgment together. So why is that not what we see?

## Organizations prefer process

Process is visible. It can be measured, reported, standardized, and scaled. Management can see the number of rounds, whether the scorecard was completed, whether the rubric was followed, whether the committee met.

Judgment is none of those things. It is invisible until exercised, hard to measure, difficult to standardize, and does not scale by adding headcount. An organization cannot purchase it, install it, or mandate it by policy.

So organizations invest in the part they can control. That is not irrational. Process answers real problems of volume, consistency, and accountability.

But process and judgment cannot scale in the same way. A screening stage can be added quickly; an evaluator who understands the work cannot. Process compounds through tools and policy. Judgment remains embodied in people. Over time, process accumulates institutional authority that judgment does not have — until it is no longer supporting judgment but displacing it.

Process determines who reaches a human evaluator. AI screens, keyword filters, and ATS systems make the first cut — before any interviewer sees a resume. Erhard illustrates the alternative: he dropped those filters, evaluated people directly, and hired seven in eight months. He could do that because his judgment did not need the filters. Most organizations cannot, because the filters are not optional — they are the process, and the process runs first.

Process determines what evidence counts. When hiring is detached from specific teams and specific work — general hiring, where the question becomes "is this person good enough to be placed somewhere later?" — evaluation defaults to generic signals: algorithms, standard puzzles, broad rubrics, prestige markers. Neigebauer described the result: asked to architect a system on the fly with no context, no requirements, no users. What the interview measured was how much system-design prep he had memorized, not how he actually approaches architecture.

Process distributes veto power. When the organization lacks confidence in its own judgment, it adds rounds. If the job is not clearly defined, add a screen. If interviewers cannot distinguish depth from fluency, add a technical round. If the hiring manager does not own the decision, add a panel. If nobody wants accountability, the loop stretches until rejection feels safer than decision. A survey of 218 U.S. recruiters hiring recent CS graduates found multi-step pipelines broadly similar to conventional hiring, with added emphasis on technical and coding tests [8]. The population was new graduates, not the whole IT workforce, but the process shape is familiar. In a qualitative study of Hacker News discussions, software practitioners described technical interviews as stressful, governed by arbitrary or obscure norms, and disconnected from real work [9]. The sample was not representative, but it documents the experience described in the five posts above. Both can be true at the same time.

Process can override judgment altogether. Neumark sent 400 applications with decades of experience across multiple stacks. Zero responses. The process ran — every filter fired, every system processed. It did not fail to execute. It failed to evaluate.

IT can arrive there by a different route. Engineering managers need not be the strongest engineers — and once promoted, the role generally takes them away from hands-on technical work [10]. Yet technical knowledge is what enables them to understand engineers' work and evaluate it fairly [10]. Their authority remains current by title; their judgment does not remain current automatically. The role gives them the power to evaluate work while progressively distancing them from the work being evaluated. When the person responsible for hiring cannot confidently judge the work, formal process gains authority by default.

The result is an inversion. The studies show why judgment is the evaluative capability. The cases above show how process can nevertheless acquire greater institutional authority. The capability on which hiring depends has become subordinate to the mechanism that was created to support it.

## The real failure: the container outranks the capability

IT hiring is not broken because companies lack process.

It is broken because process has expanded beyond its role. It was designed to contain and support judgment. It now substitutes for judgment, and it has the institutional authority to do so unchallenged.

Structure matters. Consistency matters. Rubrics matter. But the decisive capability is still competent judgment — people who understand the work deeply enough to evaluate whether another person can do it.

A framework is not a capability. It is a container for capability.

---

## References

[1] Steven Singer and Derya Oktay, "Ghost Jobs, Real Costs: Labor Market Distortions from Ghost Job Postings," *Business Economics* 60, no. 4 (2025): 216–229. The paper develops an empirical framework for identifying ghost postings and measuring their labor-market costs. https://doi.org/10.1057/s11369-025-00436-z

[2] Paul R. Sackett, Charlene Zhang, Christopher M. Berry, and Filip Lievens, "Revisiting Meta-Analytic Estimates of Validity in Personnel Selection: Addressing Systematic Overcorrection for Restriction of Range," *Journal of Applied Psychology* 107, no. 11 (2022): 2040–2068. https://doi.org/10.1037/apl0000994

[3] Julia Levashina, Christopher J. Hartwell, Frederick P. Morgeson, and Michael A. Campion, "The Structured Employment Interview: Narrative and Quantitative Review of the Research Literature," *Personnel Psychology* 67, no. 1 (2014): 241–293. https://doi.org/10.1111/peps.12052

[4] Christopher J. Hartwell and Michael A. Campion, "Getting on the Same Page: The Effect of Normative Feedback Interventions on Structured Interview Ratings," *Journal of Applied Psychology* 101, no. 6 (2016): 757–778. Over 20,000 interviews by more than 100 interviewers across more than four years. Leniency and severity differences were significant; normative feedback reduced discrepancies but lost effectiveness over time. In selection simulations, rating differences affected which applicants would have been hired. https://doi.org/10.1037/apl0000099

[5] Shanshi Liu, Yuanzheng Chang, Jianwu Jiang, Haigang Ma, and Huaikang Zhou, "Predictive Validity of Interviewer Post-interview Notes on Candidates' Job Outcomes: Evidence Using Text Data From a Leading Chinese IT Company," *Frontiers in Psychology* 11 (2021): 522830. Among 7,650 hired candidates, greater coverage of job-related capabilities in interview notes was associated with better later performance and promotion, and with lower turnover. https://doi.org/10.3389/fpsyg.2020.522830

[6] Jeff A. Weekley, Jeffrey R. Labrador, Michael A. Campion, and Kathleen Frye, "Job Analysis Ratings and Criterion-Related Validity: Are They Related and Can Validity Be Used as a Measure of Accuracy?" *Journal of Occupational and Organizational Psychology* 92, no. 4 (2019): 764–786. https://doi.org/10.1111/joop.12272

[7] Tomoki Sekiguchi and Vandra L. Huber, "The Use of Person–Organization Fit and Person–Job Fit Information in Making Selection Decisions," *Organizational Behavior and Human Decision Processes* 116, no. 2 (2011): 203–216. https://doi.org/10.1016/j.obhdp.2011.04.001

[8] Anna Stepanova, Alexis Weaver, Joanna Lahey, Gerianne Alexander, and Tracy Hammond, "Hiring CS Graduates: What We Learned from Employers," *ACM Transactions on Computing Education* 22, no. 1 (2022): Article 5, 1–20. First published online October 18, 2021. The study surveyed 218 U.S. recruiters hiring recent CS graduates. https://doi.org/10.1145/3474623

[9] Mahnaz Behroozi, Chris Parnin, and Titus Barik, "Hiring Is Broken: What Do Developers Say About Technical Interviews?" *2019 IEEE Symposium on Visual Languages and Human-Centric Computing (VL/HCC)* (2019): 1–9. https://doi.org/10.1109/VLHCC.2019.8818836

[10] Eirini Kalliamvakou, Christian Bird, Thomas Zimmermann, Andrew Begel, Robert DeLine, and Daniel M. German, "What Makes a Great Manager of Software Engineers?" *IEEE Transactions on Software Engineering* 45, no. 1 (2019): 87–106. Based on 37 interviews and 563 survey responses at Microsoft. The survey scenario contrasted a candidate who was excellent technically and competent socially with one who was competent technically and excellent socially; Section 6 reports that 75 percent selected the latter. Interviewees described engineering managers as generally no longer producing technical output, while emphasizing that sufficient technical knowledge helps them understand engineers' work and evaluate it fairly. https://doi.org/10.1109/TSE.2017.2768368
