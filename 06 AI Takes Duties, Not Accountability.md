# AI Takes Duties, Not Accountability

*Originally published on LinkedIn: <https://lnkd.in/p/g83-xYpn>*

## The single fact

Every organization runs on a quiet assumption we never had to say out loud: the worker you supervise is also, in part, accountable. Editors and writers. Managers and direct reports. Contractors and subcontractors. In every prior arrangement, the one doing the work could hold a share of the answer.

It is the first time in history an organization has given real duties to a worker that cannot bear any accountability for them. Agentic AI can perform the duty; it cannot answer for it. So accountability no longer splits the way it always has — instead, 100% of it falls back onto a human, and 0% stays with the thing that produced the work.

This is the piece I would add to an earlier post in this series. There I said judgment and accountability "remain human." That was the *what*. This is the *why*: they remain human because the only other candidate cannot hold them.

## It already has a name

This is not a prediction. It is documented. Researchers call the pattern a *moral crumple zone* [1]: when a human–automation system fails, responsibility is misattributed to the nearest human — often the one with the least real control — protecting the system, not the person.

But the crumple only bites where the human cannot realistically verify the output. That is exactly the ground this series has been mapping, and it takes two forms:

- **The output is massive.** An AI generates thousands of lines of code. A human "owns" it but cannot check every line against the intent behind it. This is the semantic boundary from the first post: there is no cheap, deterministic test that output satisfies intent, and sheer volume puts a full human check out of reach.
- **The judgment enters the fast lane.** A running system — a CI/CD pipeline asking "is this deploy safe?" — waits for the AI's judgment, then acts on it at machine speed, with no human positioned to verify each call. This is the speed boundary from the earlier posts.

And it is arriving at scale. Most enterprises expect at least moderate agent use by 2027, yet only about a fifth — 21% — report a mature way to govern it [2]. Deployment is moving faster than governance.

## Nobody's fault. Everyone's caution.

I am not assigning blame. Nobody arranged it on purpose. The accountability just settles, quietly, onto whoever is nearest — while everyone is doing exactly what they were asked.

So this is a warning, not an accusation.

The danger was never that the AI makes mistakes. It is that we keep calling this delegation, when the one thing delegation always guaranteed — a delegate who shared the answer — is precisely what is missing.

We have named the damage — blame landing on the nearest human. We have not yet named the setup that causes it: giving an AI a duty it cannot answer for. That is exactly why it is worth being careful.

---

## References

[1] Madeleine Clare Elish, *Moral Crumple Zones: Cautionary Tales in Human-Robot Interaction*, Engaging Science, Technology, and Society, 2019. <https://estsjournal.org/index.php/ests/article/view/260>

[2] Deloitte, *State of AI in the Enterprise* (2026); see "Agentic AI is scaling faster than guardrails" — 74% expect at least moderate agentic-AI use by 2027, while 21% report a mature governance model (survey of 3,235 leaders, Aug–Sep 2025). <https://www.deloitte.com/us/en/insights/topics/emerging-technologies/ai-agents-scaling-faster.html>
