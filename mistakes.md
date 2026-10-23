# Mistakes — what I got wrong and why

Reviewed with the `/mistakes` skill. Each entry is a wrong move, not a wrong fact: the thinking behind it matters
more than the correction. `Status`: **open** (not yet reviewed), **understood** (I can explain why it was wrong),
**fixed** (I did the right thing the next time it came up; note where).

| # | Day | Status | Category | Mistake (short) |
|---|---|---|---|---|
| 1 | D01 | open | Steering the client | Walked into discovery already sure that refunds need a human in the loop |
| 2 | D01 | open | Cost model | Cost estimate named volume and tokens per ticket, but missed calls per ticket, input vs output pricing, and peaks |
| 3 | D01 | open | Discovery | Called the language question "just a spec" and irrelevant for now |

## Interview cheat sheet (reread before an interview)

1. **Hypothesis, not conclusion.** Experience gives you a hypothesis ("money needs a human"). Test it with open
   questions: what happens, who decides, what amounts. Never ask "so X, right?". The client will just say yes.
2. **Ask for a story, not an opinion.** "Walk me through the last ticket that took hours" shows the real steps and
   the bottleneck. "What's wrong with support?" gets "it's slow".
3. **Ask everything on the checklist; only the order changes.** Language, channels, systems, budget, and
   success definition all matter later. Don't dismiss a question because it isn't the opener.
4. **Cost = calls × tokens × price.** Count calls per ticket first (classify, draft, check, retries), then input and
   output tokens separately (different prices), then multiply by volume.
5. **Size on the worst day, not the average.** Give three numbers: average-day cost, peak-day cost, cost if volume
   grows 50%. Peaks cause rate-limit queues and blow the budget. Growth makes today's number stale at launch.
6. **Ask for the numbers behind the numbers.** Current volume (not forecast), channel split, ticket-type mix, busiest
   day, busiest hour, and when and how fast it started growing.
7. **Don't ask for data before you need it.** Real tickets come at the data drop (D04), not before the kickoff.

---

## 1 · D01 · Steering the client

- **What I did:** Said "anything connected with money needs a human in the loop" as part of my plan for the kickoff call.
- **What was wrong:** It is a conclusion I brought with me, not something I found out. If I carry it into the call, my
  questions bend toward confirming it. The guide (16.2) says that leading the client to a pre-decided answer is what
  gets candidates screened out.
- **Right move:** Keep it as a hypothesis. Ask open questions about refunds (what happens, who decides, what amounts,
  what goes wrong) and let Arev's answers get me there. It may still end up as the same answer.
- **Why it can be wrong at Arev:** Most refunds may be tiny (e.g. a delivery fee), so a human approving each one
  could be the bottleneck itself and the real rule might be "human above X amount". Refunds may already need
  finance, not support. Or refunds may not be the main problem at all.
- **What goes wrong if I'm sure:** My questions bend toward confirming it, the client says yes, I stop hearing
  answers that contradict it, and I build the wrong thing with confidence.
- **Why I did it (to review):** Prior experience in fintech-like systems made this feel like a fact.
- **Next time:** Before a call, write hypotheses separately from questions, and ask only the questions.

## 2 · D01 · Cost model

- **What I did:** Listed daily ticket volume, tokens per ticket type, and model prices as the cost inputs.
- **What was wrong:** Missed that one ticket may cost several model calls (retrieval, draft, retry), that input and
  output tokens are priced differently, and that sizing on the average day hides peaks and the marketplace growth.
- **Right move:** cost per ticket = Σ calls × (input tokens × input price + output tokens × output price), then size
  against peak volume and growth, not the average.
- **Calls per ticket:** one ticket may need several requests: classify, draft a reply from policy or order data, check
  or retry. 3 calls cost about 3× one call. Retries and long tickets push it higher.
- **Peaks:** if the average is 400 tickets/day, a marketplace sale could send 1,500, and delay complaints could
  trigger a second wave. Sizing on 400 under-estimates cost, hits provider rate limits (tickets queue, which recreates
  the problem), and can break the spend cap. Ask for the busiest day and the busiest hour.
- **Growth:** volume rose when the marketplaces started sending traffic. If it goes 300 → 400 in three months, it may
  be 600 by launch. Ask when growth started, how fast, and whether more marketplaces are coming.
- **Deliverable:** give three numbers, not one: average-day cost, peak-day cost, and cost at +50% volume.
- **Why I did it (to review):** Thought of one ticket as one request.
- **Next time:** Count calls per ticket first, then tokens per call.

## 3 · D01 · Discovery

- **What I did:** Dismissed the language question as "just a spec, it doesn't matter now".
- **What was wrong:** It is cheap to ask and it matters later (model choice, embeddings around D18). The right call
  was not to open with it, but not to write it off either.
- **Right move:** Collect it as a fact in the call, after the example-based questions.
- **Why I did it (to review):** Prioritised the bottleneck question correctly, then over-generalised that into "the
  rest doesn't matter".
- **Next time:** Rank questions by order, not by importance. Everything on the checklist still gets asked.
