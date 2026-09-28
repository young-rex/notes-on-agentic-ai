# Calibrate: Follow the Business Value

*Originally published on LinkedIn: <https://lnkd.in/p/gKBZHfWa>*

## The gap between capability and value

New AI models now arrive every few weeks. Business value does not follow on the same schedule.

Economists have seen this before. A general-purpose technology takes time to deliver its full benefits because organizations also have to invest in new business processes, new skills, and new organizational knowledge. Those investments are hard to measure, so productivity statistics can understate the gains at first and overstate them later, tracing a J-shaped curve [1].

The question I keep asking is where learning time is best spent as AI advances. That gap is a good place to look. Better models arrive on their own. Turning them into value takes work, and that work is where time is most likely to pay off.

Two kinds of work are trying to close the gap right now: AI coding, which turns capability into useful software, and AI forward-deployed engineering (FDE), which turns capability into better business operations. On Post 28's map, they fall in the two areas where most readers will work: software engineering and business operations. In both, the intended business value is clear; what stands in the way is still being worked out.

## AI coding: from more code to useful software

The value of AI coding is not code. It is improvements that customers and employees can rely on: time saved, fewer errors, or work they could not do before. Code and pull requests are output; those improvements are the outcome.

AI has accelerated code writing. What stands between more code and more value shows up at two levels.

**One developer.** Picture a developer with a capable agent: code now arrives faster than the developer can read it closely. That code still needs an owner who understands it well enough to explain and maintain it. If the developer cannot keep up, the time saved at the keyboard can reappear later as review, debugging, and rework. Researchers who studied one company's AI mandate note that their measures miss some downstream costs of AI-generated code at scale, including "diluted ownership and understanding" [2]. The organization must provide the time and support that ownership requires.

**A whole team.** When each developer produces more changes, the team has more to review and integrate. That company had committed to doubling merged pull requests per engineer [2]. Across 802 developers and about 196,000 pull requests, output per developer reached 2.09 times its pre-mandate level by April 2026, and review load per reviewer roughly doubled. The company absorbed the extra volume partly with more human review, but mainly by shifting review onto automation: the share of pull requests with a human-written review comment fell from about 39% to about 21% [2]. Rebuilding review this way was real work, and the headline number does not show it.

Merge and revert rates held steady. The authors caution that these are "coarse, short-horizon proxies that miss defects, incidents, and maintainability," and read them as "evidence against an acute quality collapse, not a clean bill of health" [2]. A pull-request count measures activity, not business value.

Research on software delivery points the same way. DORA's 2025 report describes AI's primary role as "an amplifier, magnifying an organization's existing strengths and weaknesses," and finds that the greatest returns come "not from the tools themselves, but from a strategic focus on the underlying organizational system" [3]. A team with weak review, testing, and integration gets more of its weaknesses, faster.

So the value of AI coding depends on the work around code writing: review that keeps up, integration that holds, and code that someone can maintain after its author moves on.

## AI FDE: from a working pilot to a better-run business

A forward-deployed engineer works inside a customer's business and builds AI into how it operates. Here the value is not the agent. It is lower costs, faster turnaround, better service, and a business that adapts faster to change. A deployed agent is output; those changes are the outcome. The organizations the OECD interviewed named similar motivations: improving operational efficiency, reducing costs, addressing workforce constraints, and enabling new forms of value creation [4].

Getting from a pilot to that outcome is the hard part. KPMG's September report, citing its AI Quarterly Pulse Survey, states that more than half of companies have deployed new AI systems, but only 4% are orchestrating AI across the enterprise; it presents this as the gap between a successful pilot and a system running at scale [5]. Three things decide whether the value arrives and lasts.

**Assembling the team.** The value depends on people who understand both the technology and the business, and the role is still unsettled. KPMG calls it "one of the most talked-about yet least-understood roles in enterprise AI," with service launches, partnership announcements, and vendor pitches each defining it differently [5]. KPMG describes FDE teams that pair engineers with domain experts, who check that the team is solving a problem central to the business "rather than just an interesting technical problem," and with value modelers, who define success with the business and build concrete metrics into the system [5]. Post 28 noted that the scarce part is the mix, not the title. My reading: a team like this supplies that mix.

**Making delivery repeatable.** The value has to arrive more than once. A pilot that works in a demo still has to integrate with legacy systems, handle fragmented data, and meet security and audit requirements; KPMG calls the work that bridges this gap the "last mile" [5]. Repeatable delivery also means choosing the right problems, and KPMG recommends a "rigorous intake process" for deciding which ones get an FDE team [5].

In the OECD's interviews with 25 organizations, interviewees expected near-term adoption to concentrate where tasks are structured, where outcomes "can be checked against reliable data or system records," and where the cost of an error is bounded or reversible [4]. No organization reported running agents with unrestricted autonomy; human approval remained in place for high-stakes or irreversible actions, such as payments and deleting data [4]. For near-term deployment, start with structured tasks whose results can be verified and whose failures can be contained.

**Sustaining the improvement.** The value has to outlast the engagement. KPMG describes transferring capability back to internal teams as a key part of the method, so that solutions are "sustainable over the long haul" [5]. KPMG's case for FDEs rests partly on internal engineering teams being "already stretched thin maintaining existing systems" [5]. My reading: handover only works if those internal teams gain the capacity and expertise to maintain what they inherit.

Two cautions apply. KPMG sells FDE services, so its report is an industry view rather than independent proof. The OECD interviews describe real conditions, but by the report's own account its findings are "illustrative rather than representative" [4].

## Where your time pays off

Both kinds of work put people next to business outcomes. In both, the unresolved problems are not ones that better models solve on their own.

Three things are worth learning, whichever kind of work you do:

- **Understanding the business:** what matters, and how it is measured.
- **Judging what should change:** which work to change, and whether the result can be checked.
- **Making improvements last:** ownership, handover, and maintenance after the first deployment.

As models become more capable, generating code and prototypes gets cheaper. These three remain the work that turns that output into value.

The gap between capability and value is also where the opportunity is. Every unresolved problem in this article is work someone has to do.

**Look for work that connects advancing AI capabilities to lasting business value.**

---

## Post image

| | AI coding | AI FDE |
|---|---|---|
| Output | Code and pull requests | Pilots and deployed agents |
| Outcome (business value) | Time saved, fewer errors, work users could not do before | Lower costs, faster turnaround, better service, quicker adaptation |
| What stands in the way | Understanding, review, and integration must absorb the extra output | Assembling the right team; making delivery repeatable past the last mile |
| What makes it last | Ownership and maintenance | Handover to internal teams |

**Look for work that connects advancing AI capabilities to lasting business value.**

## References

[1] Erik Brynjolfsson, Daniel Rock, and Chad Syverson, "The Productivity J-Curve: How Intangibles Complement General Purpose Technologies," *American Economic Journal: Macroeconomics* 13, no. 1 (2021): 333–372. <https://www.aeaweb.org/articles?id=10.1257/mac.20180386>. Older than this series' usual window; cited for its core idea, which has not dated: general-purpose technologies require large, hard-to-measure complementary investments before their productivity gains show.

[2] Hao He, Shyam Agarwal, Yegor Denisov-Blanch, Pavel Azaletskiy, Sanmi Koyejo, and Bogdan Vasilescu, *AI Writes Faster Than Humans Can Review: A Longitudinal Study of an Enterprise 2x Mandate*, preprint, July 2026. <https://arxiv.org/abs/2607.01904>. A panel of 802 developers and 196,212 pull requests at one mid-sized company, January 2024 to April 2026. Adoption was not randomly assigned; the authors read their evidence as strongly implicating AI adoption and use, not as exact causal attribution.

[3] DORA, *State of AI-assisted Software Development 2025*, Google Cloud, 2025. <https://dora.dev/research/2025/dora-report/>.

[4] OECD, *Agentic AI in organisations: Early insights from practitioner interviews*, OECD Artificial Intelligence Papers, No. 65, September 2026. <https://www.oecd.org/content/dam/oecd/en/publications/reports/2026/09/agentic-ai-in-organisations_1f17d235/1257a26f-en.pdf>. Structured interviews with 25 organizations, including frontier technology developers, enterprise adopters, and government agencies. Qualitative; the findings reflect organizational perspectives rather than measured impacts.

[5] Matteo Colombo and Rachel Wagner-Kaiser, *Forward Deployed Engineers: Closing the gap between AI pilots and value*, KPMG, September 2026. <https://kpmg.com/kpmg-us/content/dam/kpmg/pdf/2026/forward-deployed-engineers-ai.pdf>. The "more than half" and 4% figures come from KPMG's AI Quarterly Pulse Survey, as cited in the report. KPMG sells FDE services; the report is an industry perspective, not independent evidence of FDE effectiveness.

## Earlier in this series

- Post 28: *Calibrate: Working on Agentic AI, or Working with It?* <https://lnkd.in/p/gS5s4eRD>
- Post 12: *The Enterprise Chose Speed. Own the Burden.* <https://lnkd.in/p/gsv6FUFr>
- Post 13: *Govern the AI, and Redesign the Work*. <https://lnkd.in/p/g-uNHNR7>
- Post 19: *IT Hiring Is Broken Because Process Outranks Judgment*. <https://lnkd.in/p/gz6Khivv>
