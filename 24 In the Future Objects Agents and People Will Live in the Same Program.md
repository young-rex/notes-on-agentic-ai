# In the Future, Objects, Agents, and People Will Live in the Same Program

*Originally published on LinkedIn: <https://lnkd.in/p/gfgHcrgm>*

A program has been a place where objects live. You create them, give them names or references, send them calls, and they answer. Languages, frameworks, and hardware have all changed since; that arrangement has not.

I think the next "program" has three kinds of tenant: objects, agents, and people. Every object, every agent, and every person has its own address in one unified address space. All three can reach one another through messages and cooperate on the same task. People have usually stood outside the program, supplying inputs through a keyboard, a mouse, or another device. Here, they have a place *in* it — addressed, reached, and answering alongside objects and agents.

This is easier to see than to argue, so I will start with an example: three friends getting a small social event done, with two agents and two objects helping. One thing to grant before you read it — every participant has a textual address, and one act, posting a message to an address, reaches any of them. That is all the machinery there is. The research behind it is cited at the end [1].

## The example

Three friends want dinner on Friday.

One address can have several *attentions*. What follows the `#` says what, inside that address, the message is for.

**People — Alice, Bob, and Charlie.** Alice's address is `alice@smith-family.com`. Her `alice@smith-family.com # DM` attention is for Alice directly. Bob and Charlie have addresses of their own, each with a `DM` attention.

**Agents — Alice's Planner and Bob's Planner.** Each agent lives behind its owner's address. Alice's is reached at `alice@smith-family.com # Planner`; Bob's at `bob@lee-home.net # Planner`. These are the two agents that participate in the example.

**Objects — Wonderland's FrontDesk and Table12.** Alice is a member of the restaurant at `restaurant@wonderland.com`. Two objects help with the booking:

- **FrontDesk**, at `restaurant@wonderland.com # FrontDesk`, answers which tables are free for a given time and party size.
- **Table12**, at `restaurant@wonderland.com # Table12`, takes a reservation record. It checks the supplied member identifier against its membership records and validates the reservation details. If those checks pass, it writes one row into a database table.

```
Message 01
From:      Alice's DM
To:        alice@smith-family.com # Planner
Reply-to:  alice@smith-family.com # DM
Text:      "Dinner with Bob and Charlie this Friday, near the office, around 7. Work out a time that suits them and book it."

Message 02
From:      Alice's Planner
To:        bob@lee-home.net # DM
Reply-to:  alice@smith-family.com # Planner
Text:      "Alice is arranging dinner Friday near the office, around 7, with you and Charlie. Does that work?"

Message 03
From:      Alice's Planner
To:        charlie@park-family.org # DM
Reply-to:  alice@smith-family.com # Planner
Text:      "Alice is arranging dinner Friday near the office, around 7, with you and Bob. Does that work?"

Message 04
From:      Charlie's DM
To:        alice@smith-family.com # Planner
Reply-to:  —
Text:      "Sorry, out of town this weekend."

Message 05
From:      Bob's DM
To:        bob@lee-home.net # Planner
Reply-to:  bob@lee-home.net # DM
Text:      "Work out a time with Alice's planner. I'm at the gym until 7."

Message 06
From:      Bob's Planner
To:        alice@smith-family.com # Planner
Reply-to:  bob@lee-home.net # Planner
Text:      "Bob is at the gym until 7, then needs to shower and get downtown. Could we make it 8?"

Message 07
From:      Alice's Planner
To:        bob@lee-home.net # Planner
Reply-to:  alice@smith-family.com # Planner
Text:      "8 is later than Alice meant by 'around 7'. Could Bob do 7:30?"

Message 08
From:      Bob's Planner
To:        alice@smith-family.com # Planner
Reply-to:  bob@lee-home.net # Planner
Text:      "7:30 works. Bob will be there."

Message 09
From:      Alice's Planner
To:        restaurant@wonderland.com # FrontDesk
Reply-to:  alice@smith-family.com # Planner
JSON:      { "date": "Fri", "time": "19:30", "party": 2 }

Message 10
From:      Wonderland's FrontDesk
To:        alice@smith-family.com # Planner
Reply-to:  —
JSON:      { "free": ["Table12", "Table7"] }

Message 11
From:      Alice's Planner
To:        restaurant@wonderland.com # Table12
Reply-to:  alice@smith-family.com # Planner
JSON:      { "member": "alice@smith-family.com", "date": "Fri", "time": "19:30", "party": 2 }

Message 12
From:      Wonderland's Table12
To:        alice@smith-family.com # Planner
Reply-to:  —
JSON:      { "reserved": "R-2291", "table": "Table12", "time": "19:30" }

Message 13
From:      Alice's Planner
To:        alice@smith-family.com # DM
Reply-to:  —
Text:      "Done. Wonderland, Friday 7:30, Table 12, you and Bob — he asked for a later start.
            Charlie is out of town."

Message 14
From:      Alice's Planner
To:        bob@lee-home.net # DM
Reply-to:  —
Text:      "Booked: Wonderland, Friday 7:30, Table 12. See you there."
```

Dinner is booked — for two, half an hour later than Alice first said. Note the party size in message 9: nobody told the planner to drop Charlie. Note too that messages 1–8 carry sentences and 9–12 carry records — the same envelope, either way. Fourteen messages, three kinds of tenant, and not one call. Everything in the rest of this piece is already in those fourteen messages.

## Three processors, three outputs

An earlier post in this series put CPUs, LLMs, and brains beside one another: three systems, each with a bounded context window and a larger stack built around it. Here the question is what each contributes when those stacks meet.

Look at what each produced above. The restaurant's code took a date, a time, and a party size and produced a list of two free tables (10); then took reservation details, checked them against its records and rules, wrote a row, and produced a reservation number (12). Alice produced an order (1); Charlie produced "Sorry, out of town" (4). Two agents took one person's "around 7" and another person's "gym until 7" and produced a time neither person had said: 7:30 (6, 7, 8).

Three kinds of processing — code execution, model inference, human thought. Three outputs that could not look more different: a table list, an apology, a negotiated time.

And all three arrived at one attention, `alice@smith-family.com # Planner`, and were used the same way: a person's apology (4), another agent's proposal (6), an object's record (10), folded into one decision by something that never had to know which processor made which. That is the abstraction this piece rests on. Below it, a CPU produces a computational result. At it, the same result is a piece of intelligent information like any other, and its producer has an address like any other. It runs in one direction only: it does not pull the model and the person down to arithmetic, it lifts the CPU stack up to stand level with them. The argument for it is in [1]. Here it is enough that you just watched it work.

## Everyone has an address, and it is text

Read the *To* line of every message in the example. `alice@smith-family.com # Planner`. `restaurant@wonderland.com # Table12`. `charlie@park-family.org # DM`. Two agents, two objects, and three people, and you cannot tell from the address form which is which. Each is a name, then an attention. An agent is an attention on a person's address; an object sits behind an address of exactly the same form.

Text, and only text — because a bare string commands nothing, and so anything that can read can carry it: a runtime, a router, a mail sorter, a person with a pen. That helplessness is what lets one address form span three processors that share no runtime, no transport, and no type system. The addressing model behind the example, Lemina, was derived from exactly that constraint [1].

## People do not fit in a program — so an avatar stands in

Now the objection is physical, and it is right: a person cannot fit into the digital world. Neurons do not run on a server. Whatever sits at `alice@smith-family.com`, it is not Alice.

It is her avatar — a digital body she owns: an address on a domain that is hers, and behind it a set of software organs she chose. `DM` is one of those organs; `Planner` is another. Look at messages 6, 7, and 8: nobody typed those. Bob was at the gym; Alice had said her piece in message 1 and gone back to work. Three messages in the example were typed by a person — 1, 4, and 5. Everything else was written by an avatar under an authorization its owner gave, or by the restaurant's code. Being addressable does not mean being present.

Email has worked this way for fifty years: the mailbox is in the system; the person is not. The avatar is an email address that grew organs. I have written about that body separately [2].

## One act reaches all three

Post a message to an address, and walk away. The carrier accepts it, and that acceptance creates no further obligation to the sender — no result, no reply, no confirmation that anything happened. Every line in the example is that act and nothing else. Where an answer was wanted, the message carried an address for it — `reply-to` — and the answer, when it came, was a new message making its own one-way trip. Message 10 is not the return value of message 9. It is a letter, sent back to the address message 9 named.

That is why the timings do not matter. The objects answered in microseconds (10, 12). The two agents answered each other in seconds (6, 7, 8). Charlie answered when he happened to look (4). Bob did not answer at all — he handed the question to his Planner (5) — and could have phoned Alice instead, which the program never sees. Not one of those is a failure. A postal service that sometimes loses a letter is not a broken instance of this shape; it is one of the oldest and truest instances of it.

The sender writes the same kind of message to an object, an agent, or a person, and is never required to know which it was. Where a tenant cannot receive on its own — a passive object that has to be invoked, a person who is at the gym — the machinery around it does the receiving: wakes it, holds for it, speaks for it. The field is leveled at the address, and only there.

## They do not sit side by side. They nest.

I drew the three tenants as if they were three houses on one street. They are not. Open any door and the street is inside.

Go back to what happened behind `alice@smith-family.com # Planner` between message 1 and message 13. Code received each message first. It could parse the records in 10 and 12 and could not parse the sentences in 4, 6, and 8. An AI read those: it understood that Charlie was out, that Bob wanted 8, and that "around 7" — Alice's words — stretched to 7:30 but not to 8 (7). Nobody specified that; it was judged. It did not wake Alice for any of it, because message 1 had authorized it. What it would not have decided alone — "Bob wants to move it to Saturday" — would have gone to her DM, and the program would have waited. Three phases behind one attention: code, then an AI, then a person on escalation, each catching what the one before it dropped.

Behind Bob's door the same three phases ran in a different order: his DM's code received message 2, Bob himself read it, and his Planner took it from there (5). Behind the restaurant's `FrontDesk` and `Table12`: code, and only code — for now.

Behind every door there is another world with its own rooms, and each room may hold all three tenants again. The program does not legislate that depth. It stops at every door, and because it stops at every door, it can stand at any door.

## What the program does not do

Earlier in this series, I traced the CPU stack's foundation to specification and the LLM stack's to judgment. Those distinctions still hold when the participants share a program.

The program unifies reaching. It does not unify trusting.

The free-table list can be checked against the booking records and a specification. "Sorry, out of town" comes from someone who can answer for it. The 7:30 in message 7 is neither: code can check its shape and not its judgment, and the agent cannot answer for itself. Treating those outputs as one kind of thing for carrying is not treating them as one kind of thing for believing. The door is level; the worlds behind it keep their nature. When Bob's Planner speaks for Bob (6, 8), the accountability stays with Bob, who told it to (5). When Alice's Planner moves dinner by half an hour (7) and books a table in Alice's name (11), it stays with Alice, who ordered it (1). Verification — who sent a message, and whose authority they carry — lives on the receiving side, behind the door, never in the post.

The same address does not make objects, agents, and people the same kind of participant. It makes them reachable the same way. That is all it does — and it is enough, because reaching is the part nobody had unified, and trusting was never the carrier's job.

## What you will recognize

None of this is new. Post offices are this. Email is this. The shape was in the world long before anyone modeled it. What is new is who moves in. Read one way, two new tenants — objects and agents — are moving into a shape people have used for centuries. Read the other way, one new tenant — people — is moving into a shape programs have used for fifty years. Both readings are the same program.

I cannot show it to you. I have shown you fourteen messages and told you what to look for: one address form for objects, agents, and people; one act of sending; a carrier that reads the envelope and never interprets the payload; worlds behind every door, and the same three tenants inside each of them.

When it is built, come back to this page and check.

---

## References

[1] Rex Young, *Lemina Addressing Metamodel*, 2026. The addressing model the example runs on: textual addresses, one-way messages carried by a mediated postal service, and a layered design that fixes only those commitments and leaves the rest open by name. <https://doi.org/10.5281/zenodo.20680627>

[2] Rex Young, *Lemitar: A Vision for Owned Digital Presence*, working draft, 2026. The avatar behind a person's address: an identifier the person owns, a body of software features the person assembles, and the three phases — code, GenAI, human — through which a message reaching that address is handled. <https://github.com/young-rex/lemina/blob/main/downstream/lemitar.md>

## Earlier in this series

- [LLMs, CPUs, and Brains All Have a Context Window. LLMs Are the Third Time We Face It.](https://lnkd.in/p/gRTeyzey)
- [Some Work Can Be Spelled Out. The Rest Has to Be Trusted to Someone.](https://lnkd.in/p/gUrzSnVc)
- [The CPU Stack Runs on Specification. The LLM Stack Runs on Judgment.](https://lnkd.in/p/ga3apNfd)
