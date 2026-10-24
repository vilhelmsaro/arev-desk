# Mistakes — what I got wrong and why

Reviewed with the `/mistakes` skill. Each entry is a wrong move, not a wrong fact: the thinking behind it matters
more than the correction. `Status`: **open** (not yet reviewed), **understood** (I can explain why it was wrong),
**fixed** (I did the right thing the next time it came up; note where).

| # | Day | Status | Category | Mistake (short) |
|---|---|---|---|---|
| 1 | D01 | open | Steering the client | Walked into discovery already sure that refunds need a human in the loop |
| 2 | D01 | open | Cost model | Cost estimate named volume and tokens per ticket, but missed calls per ticket, input vs output pricing, and peaks |
| 3 | D01 | open | Discovery | Called the language question "just a spec" and irrelevant for now |
| 4 | D01 | open | Decision-making | Picked interviewees without asking what the choice leaves unknown (no IT lead) |
| 5 | D01 | open | Steering the client | Proposed solutions on the call (ticket system, email to finance, single source of truth) before understanding the process |
| 6 | D01 | open | Discovery | Ended the kickoff without asking for success definition, budget signal or languages |
| 7 | D01 | open | Cost model | Mixed up data lookups (Sheets, Gmail, ArevAdmin) with LLM calls |

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
8. **For every option, ask what it leaves unknown.** "Most valuable" isn't a reason. Name what you won't learn
   (e.g. systems, access, constraints) and check whether that blind spot can kill the project.

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

## 4 · D01 · Decision-making

- **What I did:** Picked "support lead, frontline agent, refund approver" as the three interviews because it felt "the most
  valuable", without giving a reason beyond that.
- **What was wrong:** I judged the option by what it gives, not by what it leaves out. It had no IT lead, so nobody would
  tell me about systems, data access, login or hosting rules, and deployment windows. Those constraints are what decide
  whether a pilot is approved (guide 16.5) and are easy to discover too late.
- **Right move:** For each option, write down what I will not learn, and ask whether that gap can kill the project. The
  full set should cover: how the work is done, where the money goes, and what the systems allow.
- **Why I did it (to review):** Anchored on the people closest to the work and the money, which matched my instinct.
- **Next time:** Before picking, list for each option "what I won't know". If a blind spot covers systems, data or
  constraints, add someone who covers it. The final pick, support lead + IT lead + finance, is decision 0001.

## 5 · D01 · Steering the client

- **What I did:** On the kickoff call I proposed a ticket system or an email to finance to speed up refunds, and a
  single source of truth, before I understood how refunds are handled or why they are slow.
- **What was wrong:** It is the same habit as #1: bringing a solution in before the facts. The client reacted to my
  idea, not to her own problem, and Ani pushed back ("talk to Narek first"). Solutions also anchor what she thinks
  she is buying.
- **Right move:** Ask what happens today and where it hurts, and keep the idea for later, after interviews and
  numbers.
- **Why I did it (to review):** Backend instinct to fix a bottleneck as soon as I see it.
- **Next time:** In discovery, only questions. Write ideas on a separate "later" list.

## 6 · D01 · Discovery

- **What I did:** Closed the call saying "I don't think I have anything else" without asking how she defines
  success, what the budget range is, or what languages tickets come in.
- **What was wrong:** Those were on the D01 goal list (and on the cheat sheet, rule 3). Without a success definition I
  cannot scope on D03, and without a budget signal I cannot size a solution.
- **Right move:** Before ending, run the checklist silently: volumes, channels, languages, systems, budget signal,
  success definition. Ask for what is missing even if time is short.
- **Why I did it (to review):** The call was running out of time and I focused on the technical follow-ups.
- **Next time:** Keep the checklist in front of me during the call and use the last two minutes on the gaps.

## 7 · D01 · Cost model

- **What I did:** Asked whether the LLM calls are "calls to Google Sheets, Gmail and the admin system".
- **What was wrong:** Those are data lookups (reads from a source). LLM calls are requests to the model. They are
  related, because lookup results go into the prompt and add input tokens, but they are priced differently and counted
  separately.
- **Right move:** Cost per ticket = LLM calls (tokens in and out × model price) plus lookup effort (usually cheap or
  free, but adds tokens to the prompt). Map the lookups, then count model calls.
- **Why I did it (to review):** Thought of each "step" in the process as one call.
- **Next time:** Draw the pipeline and mark which steps call a model and which only read data.
