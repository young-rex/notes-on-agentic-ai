# 41. From LLM Calls to LLM Programs

*Originally published on LinkedIn: <https://lnkd.in/p/gNyPnB4U>*

## An architecture, not a kind of use

Post 34 defined the LLM stack's one processor-level operation, the LLM call: one inference over a context the caller supplies. It also defined the first stack-level operation, the agent call, and set it against a function call in two sentences: "in code, every next step follows rules written before the program runs. In an agent call, the model can pick the next step while it runs."

Public attention goes mostly to chat, where a person picks each next step, and to agents, where the model does. Here I take the code side of Post 34's distinction: application code composes LLM calls and decides what can follow what, the way a program arranges CPU instructions. Schlag et al. call the result an LLM program [1], and the series uses that term from here on. Post 34 named the unit; this post names the composition. What sets an LLM program apart is its architecture, not its use: the same task could go to an agent instead.

This post asks one question: what makes a set of LLM calls an LLM program? The next posts ask what useful work LLM programs do, when splitting work across calls helps, and what a program can enforce.

## Who defines the procedure, and who selects the path?

Post 32 asked who owns the loop in C3 software, which mixes code and language. In code-led C3, "code owns the loop and calls the LLM for one step, such as extraction, routing, or review." In LLM-led C3, "the LLM owns the loop and calls code as tools." For LLM programs, the question splits in two, because a model's output can steer a loop it does not own:

- **Who defines the procedure:** the control-flow map—the available steps, branches, loops, and stopping conditions.
- **Who selects a path at runtime:** the route a particular run takes through that map.

In an LLM program, code defines the procedure before the run, and a model's output can select a path only at points the code declares. Post 34 used C's **strlen(s)** to show a function call with a loop inside: the code defines the loop, and the string decides how many times it runs. A model's output plays the string's part. It can pick a branch or end a loop, but it cannot add a step the code does not contain.

An agent's harness is code too, but it defines little of the procedure: call the model, run the tool it asks for, and repeat until it answers or hits a limit. The model generates and orders the steps as it goes. Harnesses differ in how much of the procedure they write into code, so real systems fall along a range, and the test applies at each level. If a model generates the task's steps and their order for code to execute, the model defines the procedure at that level. A program can hand one of its steps to an agent call and still be a program at its own level.

In Schlag et al.'s LLM programs, a task is broken into steps a model can perform, "strung together by a classic computer program" that "uses control flow" [1]. Anthropic draws the same line between workflows, orchestrated "through predefined code paths," and agents [4]. I use Schlag et al.'s term because it keeps the parallel with the CPU stack, where programs arrange instructions.

**In an LLM program, code defines the procedure. A model's output can select a path, but only where the code declares one.**

## What does an LLM program look like?

The structured program theorem, credited to Böhm and Jacopini's 1966 paper, holds that sequence, selection, and iteration suffice for any program's control flow [2]. LLM programs use the same constructs, sometimes with parallel work added. What changes is that some conditions come from LLM calls.

The example is purely illustrative: shorten a customer notice to 120 words while keeping certain required sentences word for word. Here is the program, in Python with Anthropic's SDK [3]:

```python
import anthropic
from pydantic import BaseModel

client = anthropic.Anthropic(timeout=120.0, max_retries=2)

class CallFailed(Exception):
  """A call stopped early or returned no valid verdict."""

def call(system, user):
  """One LLM call: one context in, one response out."""
  response = client.messages.create(
    model="claude-opus-5-5",
    max_tokens=16000,
    system=system,
    messages=[{"role": "user", "content": user}],
  )
  if response.stop_reason != "end_turn":
    raise CallFailed(f"Call stopped early: {response.stop_reason}")
  return "".join(b.text for b in response.content if b.type == "text")

class Verdict(BaseModel):
  accept: bool
  reasons: list[str]

def review(notice, draft):
  """A call whose output code acts on, so the SDK validates its format."""
  response = client.messages.parse(
    model="claude-opus-5-5",
    max_tokens=16000,
    system=REVIEW,
    messages=[{"role": "user",
      "content": f"ORIGINAL:\n{notice}\n\nSHORTENED:\n{draft}"}],
    output_format=Verdict,
  )
  if response.stop_reason != "end_turn" or response.parsed_output is None:
    raise CallFailed(f"Review failed: {response.stop_reason}")
  return response.parsed_output

def shorten_notice(notice, required, limit=120, attempts=3):
  draft, feedback = None, ""
  try:
    for _ in range(attempts):
      draft = call(SHORTEN, request(notice, required, limit, draft, feedback))
      problems = check(draft, required, limit)
      if not problems:
        verdict = review(notice, draft)
        if verdict.accept:
          return "accepted", draft, ""
        problems = verdict.reasons
      feedback = "\n".join(problems)
  except (anthropic.APIStatusError, anthropic.APIConnectionError,
          CallFailed) as error:
    return "failed", draft, str(error)
  return "unresolved", draft, feedback
```

The sequence is draft, check, review. The branch, accept or revise, rests on two conditions of different kinds. The check is a specified rule: code can count words and match fixed sentences completely. The verdict is a judgment, made by another call. Because code acts on it, it comes back in a format the SDK validates. Three attempts allow a first draft and up to two revisions, each given the previous draft and the reasons it failed. Parallel work would review several drafts at once; routing could send regulatory notices straight to a person.

Apply the two questions. A developer defined the procedure, in code. At runtime, the check and the verdict select the path, at the points the code declares. Post 10 placed the LLM stack's judgment in "context selection, loop termination, and graph routing." Here judgment routes the program and can end the loop, but it can only choose between accepting and revising.

Where does each part reside? Instructions live in each call's context. State lives in the program, in **draft** and **feedback**, and reaches a call only through the context the program composes. As Post 34 put it, "The state of the work stays on the caller's side." Control lives in the loop and its branches.

The program maps the outcomes it handles to three declared states, and each claims only what the run established:

- **accepted**: the declared checks passed, both the code check and the review.
- **unresolved**: the attempts ran out. That does not prove that no acceptable draft exists.
- **failed**: a call raised an API or connection error, stopped early, or returned no valid verdict.

Errors outside those cases still reach the caller. The configured limits are three attempts and, for each request, a timeout with up to two retries. The request timeout is not a deadline for the whole run [3].

**LLM programs use the old constructs. What is new is that an LLM's judgment can decide a branch or end a loop.**

## Where do LLM programs fit?

Post 34 left room for more operations: "The stack level has its first, and more can follow." The LLM program suggests the next one, once the composition is kept apart from its invocation.

**The LLM program is the composition**: the calls, the code between them, and the control flow that joins them. **A program call exposes that composition to a caller as one operation.** To its caller, **shorten_notice(notice, required)** looks like an agent call: a task goes in, and a result comes back. Underneath, they differ in who defines the procedure. Give a model the same objective and a word-counting tool, let it decide the order of steps and when to finish, and you have an agent call. The program call follows a procedure written before it runs.

An ordinary function call can implement a program call. The term names what it invokes: an LLM program. Its code can enforce executable checks, such as the word limit, but what depends on LLM judgment has only a partial contract, in Post 10's sense. **I propose the program call as the second stack-level operation, next to the agent call.** **The two nest both ways**: a program can hand a step to an agent call, as noted earlier, and an agent can use a program call as a tool.

LLM programs can be C2 or C3, two of Post 32's three categories: in C2, the LLM does the business work and code only supports it; in C3, code and the LLM each carry part of it. Post 32's test asks of any piece of code: "would it still mean something to the business if the model were removed?" Orchestration alone would not. It is supporting code, so an LLM program whose code only orchestrates its calls is C2. The notice program is C3, because its code also enforces business requirements: the word limit and the required sentences. In Post 32's words, "judgment crosses into business logic."

**The LLM program is the composition, and a program call is how a caller invokes it. Whether it is C2 or C3 depends on whether its code carries business rules.**

## My reading

Three points, mine rather than the sources'.

**Old constructs, new material.** Post 9 said that when a stack is young, old problems get new names. Sequence, selection, and iteration predate LLMs, and programs have long branched on decisions people make. What is new is that LLM calls make some of those decisions.

**Defined before the run.** A model's output can steer an LLM program without making it an agent. What matters is whether the procedure exists before the run. If it does, it can be read, reviewed, and tested like other code. The judgments inside it can also be tested against cases and reviewed by people, but the LLM calls that make them have only partial contracts.

**A name, not a verdict.** Post 7 noted "No settled way to structure an agent program." The LLM program does not settle that, and naming it does not show that it beats an agent loop. It names one structure precisely enough for the rest of this mini-series to examine.

**An LLM call is the unit. An LLM program composes calls under code, and a program call lets a caller use the whole as one operation.**

---

## References

[1] Imanol Schlag, Sainbayar Sukhbaatar, Asli Celikyilmaz, Wen-tau Yih, Jason Weston, Jürgen Schmidhuber, and Xian Li, *Large Language Model Programs*, arXiv:2305.05364, May 9, 2023. <https://arxiv.org/abs/2305.05364>. Preprint; an illustrative evidence-supported question-answering program. Section 2 places state and control flow in the surrounding program.

[2] Corrado Böhm and Giuseppe Jacopini, "Flow Diagrams, Turing Machines and Languages with Only Two Formation Rules," *Communications of the ACM* 9, no. 5 (May 1966): 366–371. <https://doi.org/10.1145/355592.365646>. Usually credited as the basis of the structured program theorem; the familiar three-construct statement is a later simplification of the paper's result.

[3] Anthropic, *anthropic-sdk-python*, GitHub repository, accessed November 2026. <https://github.com/anthropics/anthropic-sdk-python>; and, in the Claude Platform documentation, *Python SDK* <https://platform.claude.com/docs/en/cli-sdks-libraries/sdks/python>, *Structured outputs* <https://platform.claude.com/docs/en/build-with-claude/structured-outputs>, and *Handling stop reasons* <https://platform.claude.com/docs/en/api/handling-stop-reasons>. The SDK applies its timeout to each request and retries timed-out requests twice by default, so the timeout is not an overall deadline. The example is illustrative and has not been run against the live API.

[4] Erik S. and Barry Zhang, *Building effective agents*, Anthropic, December 19, 2024. <https://www.anthropic.com/engineering/building-effective-agents>. Guidance from a model vendor.

## Earlier in this series

All posts are archived in the GitHub repository: <https://github.com/young-rex/notes-on-agentic-ai>

- Post 34: *Defining the LLM Stack: One Operation and Messages* <https://lnkd.in/p/gBRCT5R6>
- Post 32: *AI Coding Has Two Sides. Ours Starts with What Runs the Software.* <https://lnkd.in/p/gacN7SgW>
- Post 10: *The CPU Stack Runs on Specification. The LLM Stack Runs on Judgment.* <https://lnkd.in/p/ga3apNfd>
- Post 9: *When a Stack Is Young, Old Problems Get New Names* <https://lnkd.in/p/geCqmmfe>
- Post 7: *We Built the World Around the CPU. We Are Doing It Again Around the LLM.* <https://lnkd.in/p/gZ4QkT-Z>
