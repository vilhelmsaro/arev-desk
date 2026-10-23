# 0001 — Kickoff call plan

- **Day:** D01 · **Guide:** Part 16.1, 16.2, 1.2 · **Status:** accepted

## Question
What do we learn on the kickoff call, who do we interview next, and what do we not promise yet?

## Context
Ani Petrosyan (COO, Arev Express) asked by email for a chatbot on the website and Telegram that also handles refunds.
A vendor failed last year. Volume is rising since the marketplaces started sending traffic. She will bring the support
lead. No numbers yet.

## Options
| Decision | Option | Cost | Risk | Makes harder later |
|---|---|---|---|---|
| Learn | A. Breadth checklist | Hits every item | Shallow, feels like an interrogation | Misses the real bottleneck |
| Learn | B. One story first, then gaps | Some items unanswered | One story may not be typical | Follow-up needed for gaps |
| Learn | C. Failed vendor first | Little operational data | Hour spent on politics | No numbers for D03 scope |
| Interview | A. Support lead, frontline agent, refund approver | Weak on systems | No view of data access or constraints | Integration surprises late |
| Interview | B. Support lead, IT lead, finance | No one answering tickets all day | Misses day-to-day detail | Needs a frontline example from the support lead |
| Interview | C. Support lead, IT lead, failed-vendor person | Politically sensitive | Ani may not allow it | Opens the old failure |
| Promise | A. Nothing but a date | Feels cautious | She may want more | — |
| Promise | B. A ballpark | Anchors on unmeasured numbers | Wrong price or timeline | Hard to walk back |
| Promise | C. Evaluate, not build | Vague unless deliverables are named | Sounds like stalling | Must state what each assessment delivers |

## My pick and why (written before seeing Claude's view)
- Learn: B, "the most valuable thing I can get".
- Interview: A, "the most valuable", no further reasoning given.
- Promise: C, "I am not sure now how much time or effort it will take".

## Claude's view / pushback
- Learn: agree with B. Ask "is that typical?" after the story, keep the last 10 minutes for checklist gaps, send
  leftovers by follow-up email.
- Interview: disagreed with A because it leaves out the IT lead (systems, access, constraints, guide 16.5).
- Promise: agree with C, but name what each assessment delivers and add a date.

## Decision
- Learn: **B** — one story first (the last ticket that took hours), then the checklist gaps.
- Interview: **B** — support lead, IT lead, finance. Changed from A after pushback.
- Promise: **C** — promise to evaluate chatbot answers and refunds separately, with a stated deliverable and a date.
  No scope, price or timeline.

## How we'll know it was right
Debrief on D01: we got volumes, channels, languages, systems, a budget signal and her definition of success, and we
did not steer her toward an answer.

## Revisit if
The call shows the real problem is outside support (e.g. delivery operations), or Ani refuses access to the IT lead.
