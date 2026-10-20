# Arev Desk — the plan

46 sessions in 16 phases, plus interview drills (🎤) about twice a week.
At 2 hours/day (Core only) that's ~100 hours including drills, or about 7 weeks of daily sessions. At 4–5 hours/day (Core + Stretch)
it's ~180 hours, closer to 8–9 weeks, because the stretch work goes deeper rather than adding days.

Each day lists: **Read** (guide sections) · **Decide** (what you choose) · **Build** (what Claude builds) ·
**Verify** (the check you run) · **Stretch**.

---

## Why this order (and where it differs from the guide's order)

The guide is in *learning-dependency* order: each part only uses earlier parts. This plan keeps that rule for every
concept, but a real project is built in *engagement order*, and three things have to move:

1. **FDE discovery and scoping (16.1–16.4) come first, not last.** In a real engagement you can't pick a triage
   taxonomy, a success metric or a budget before you've talked to the client. These sections depend on nothing
   technical except token cost (1.2), which you read the same day. The rest of Part 16 (integration, demo, handoff)
   stays at the end, where it depends on a working system.
2. **Evals split in two.** Error analysis, the golden set, code assertions and the gate (9.1–9.4, 9.8) depend only on
   having outputs (Part 3), so they come right after the first pipeline works. The guide itself says to build the
   golden set before tuning anything (8.4), and your OSHA RAG stalled exactly because the eval set came last. Judges,
   RAG metrics and trajectory evals (9.5–9.7) need RAG and agents, so they come after those.
3. **Observability splits in two as well.** Basic tracing, cost attribution and logs (10.1–10.3, 10.5–10.6) go in
   early, so every later day can be debugged and costed. The agent/RAG span tree (10.4), evals↔traces (10.7) and the
   debugging drill (10.8) come once there's something to trace.

Everything else follows the guide's order: Parts 1 → 2 → 3 → 4 → 5 → 6–8 → 11 → 12 → 15, then 13–14 as the optional
model layer. Within Part 5, the framework port (5.3) comes *after* you've written a plain loop (5.1) and made the
workflow-vs-agent call (5.2), and HITL/durability (5.6–5.7) comes after RAG, when the agent has something worth pausing.

If an interview shows up early, the guide's 30-day cram order (22.4) still applies. Jump to D17, D24–26, D32 and D11
in that order, then come back.

---

## Phase 0 — The engagement starts (Part 16.1–16.4, 1.2)

### D01 · Kickoff call
- **Read:** 16.1, 16.2, 1.2 (for the cost questions). Then `engagement/00-first-email.md`.
- **Decide:** what you need to learn in this call; who you need to talk to next (you get three interviews tomorrow);
  what you will *not* promise yet.
- **Do:** 45–60 min role-play kickoff with the sponsor (Claude in character). You lead it.
- **Verify:** debrief. Did you get volumes, channels, languages, the systems involved, a budget signal and the
  sponsor's definition of success? Did you steer her toward an answer? (The guide warns that leading the client is
  what gets screened out.)
- **Stretch:** draft your interview guide for D02.

### D02 · Discovery interviews
- **Read:** 16.2 again, this time as a checklist.
- **Decide:** the problem statement *with numbers*; the top 3 problems ranked by impact; the open questions.
- **Do:** three role-play interviews: support lead, IT lead, finance. Ask for examples, not opinions ("show me the
  last ticket that took 20 minutes").
- **Build:** fill in `engagement/discovery.md`: personas, ranked problems, data map, constraints, open questions.
- **Verify:** someone who wasn't in the room can read the discovery doc and say what hurts and how much.
- **Stretch:** a second round with one stakeholder on whatever turned out to be the surprise.

### D03 · Scope the MVP
- **Read:** 16.3, 16.4.
- **Decide (ADR-0001):** the one workflow to build first; success metric and target; what's explicitly out of
  scope; POC or pilot first; a cost-to-run estimate in AMD/month.
- **Do:** a short role-play to get the sponsor's sign-off. She'll push for more scope. Practise "scope down, never slip."
- **Build:** `engagement/scope.md`.
- **Verify:** the scope doc names its cuts, and the sponsor (in character) agrees to the metric.

## Phase 1 — Ground truth and plumbing (Part 1)

### D04 · The data drop
- **Read:** 1.1. Skim the guide's "map of the whole field".
- **Decide (ADR):** how realistic the synthetic data should be, given what discovery found (languages, translit,
  duplicates, missing tracking codes, stale help articles); the repo stack (one ADR, quickly: it's plumbing).
- **Build:** the client's "data drop": orders and tracking DB (seeded Postgres standing in for their MySQL replica),
  partner tracking feeds, the help center (HY/RU/EN), the refund policy PDF, a ticket export of ~600 tickets. Plus
  the repo skeleton (uv, FastAPI, compose, pytest).
- **Verify:** you open 20 random tickets and say whether they feel real. If they don't, adjust the generator.

### D05 · Calling models properly 🎤
- **Read:** 1.2–1.5.
- **Decide (ADR):** which three labs (all via OpenRouter); which model per tier (cheap/balanced/flagship), with exact pinned IDs and
  prices checked today; temperature for triage; concurrency limit; retry policy.
- **Build:** a provider-agnostic model client (one interface; OpenRouter behind it, a direct SDK as a second adapter only if you decide it's worth it), retries with exponential backoff and
  a cap, a concurrency limit, a per-call token/cost/TTFT/total-latency record, and a versioned price table.
- **Verify:** run 100 tickets through each tier with max 20 in flight. Read the table: cost per 100, p50/p95 latency,
  TTFT. Simulate a 429 and watch it recover. This is the classic take-home exercise; time yourself explaining it.
- **🎤 Drill:** decomposition case #1 (not Arev).

## Phase 2 — The triage pipeline (Parts 2.1–2.2, 3)

### D06 · The triage prompt as a spec
- **Read:** 2.1, 2.2.
- **Decide (ADR):** the ticket taxonomy (from discovery, not invented); what "urgent" means for Arev; reply-language
  policy (customer's language? translit Armenian → which script?); identity + job + rules + output-format layout.
- **Build:** `prompts/` with a versioned triage system prompt, few-shot examples chosen by you, and data in delimiters.
- **Verify:** 30 tickets by hand. Note anything that looks wrong; you'll formalise it on D08.

### D07 · Structured triage output
- **Read:** 3.1, 3.2.
- **Decide (ADR):** the Pydantic schema (fields, Literals, bounds, an "unknown" option, `needs_human` rules); the retry
  cap and what happens after it; instructor or Pydantic AI for plain calls.
- **Build:** a triage function that returns a validated model, with a retry-with-error path and a test that forces a
  validation failure.
- **Verify:** run all 600 tickets. What % validated first time, after a retry, never? What did that cost?
- **Stretch:** extract the tracking code and order ID as fields too, and measure how often it gets them wrong.

## Phase 3 — Measure first (Parts 9.1–9.4, 9.8)

### D08 · Read the outputs
- **Read:** 9.1, 9.2.
- **Decide:** the failure categories. These come out of *your* notes, not a library.
- **Do (you, not Claude):** read 60–100 triage outputs and write one line per problem. This is the one day that's
  mostly manual on purpose; the guide's whole point is that most teams skip it.
- **Build:** a tiny review UI or a CSV workflow, if it makes the reading faster.
- **Verify:** a ranked list of failure categories with counts.

### D09 · Golden set v0 + the gate 🎤
- **Read:** 9.3, 9.4, 9.8.
- **Decide (ADR):** what goes in the golden set (50 items: easy, hard, edge, "should escalate", adversarial,
  one per failure category); the pass threshold; the fast-subset vs full-suite split.
- **Build:** `evals/golden/triage.jsonl`, code assertions in pytest, an eval report writer, and a gate script that
  fails below the threshold.
- **Verify:** break the prompt on purpose, watch the pass rate drop and the gate block it, and point to the failing items.
- **🎤 Drill:** eval design ("how do you know your triage works?").

## Phase 4 — Make it cheap and visible (Parts 2.3–2.4, 10.1–10.3, 10.5–10.6)

### D10 · Caching and the context budget
- **Read:** 2.3, 2.4.
- **Decide (ADR):** prompt layout (stable → variable); a token budget per triage call; what to cut.
- **Build:** cache markers / provider caching, plus a cache-hit counter. (Check today how OpenRouter passes caching through per provider: explicit markers for some, automatic for others.)
- **Verify:** cache hit rate, cost before/after on 600 tickets, and the eval pass rate unchanged (or you don't ship
  the cut). The guide's test: cut the prompt substantially without losing accuracy.

### D11 · Tracing the pipeline
- **Read:** 10.1, 10.2, 10.3, 10.9.
- **Decide (ADR):** the backend (Langfuse self-hosted vs Phoenix vs Logfire); which instrumentation packages
  (`-v2`); whether to normalise attribute names now; prompt-capture flag defaults.
- **Build:** the OTel SDK with resource and batch exporter, FastAPI/DB/HTTP auto-instrumentation, `gen_ai.*` spans on
  every model call, and the backend in compose.
- **Verify:** pick one request, open its trace, and read tokens, cost, model and latency off the spans.
- **Stretch:** the attribute-normalising span processor (the guide's "senior move").

### D12 · Cost attribution + structured logs 🎤
- **Read:** 10.5, 10.6.
- **Decide:** which dimensions to tag (feature, channel, language, model tier); the alert threshold against the scope doc's budget.
- **Build:** a per-request cost roll-up, structlog with trace IDs, a "cost by feature/language" view.
- **Verify:** answer "which language costs us most per ticket, and why?" from the dashboard alone.
- **🎤 Drill:** take-home walkthrough. Defend D05–D12 as if they were a take-home you submitted.

## Phase 5 — Tools (Part 4)

### D13 · Designing the tools
- **Read:** 4.1, 4.2, 4.3.
- **Decide (ADR):** the tool set (as few as possible: e.g. `get_parcel_status`, `find_orders_by_phone`, …); what each
  returns (the minimum useful fields); error messages written for the model; how to handle the partner feed that's up
  to 24h stale.
- **Build:** typed tools that read the order/tracking DB, returning Pydantic models, with tests.
- **Verify:** run 30 WISMO tickets through a single tool-calling step. Read the traces: did it pick the right tool
  with the right arguments? How many tokens did the tool results add?

### D14 · Least privilege + the refund tool
- **Read:** 4.4.
- **Decide (ADR):** read vs write tools; a scoped DB role; the refund/compensation tool's limits (it *proposes* and
  never pays); the worst-case table per tool.
- **Build:** a read-only DB user for read tools, a `propose_compensation` write tool that only creates a pending
  record, and tool-call logging with an argument hash and result size.
- **Verify:** for each tool, state the worst case under malicious arguments and show the test proving it's bounded.

## Phase 6 — The agent (Part 5.1–5.4)

### D15 · Workflow or agent?
- **Read:** 5.1, 5.2.
- **Decide (ADR, a big one):** which ticket types get a fixed workflow and which get an agent step; the iteration cap;
  the budget; the stop reasons.
- **Build:** a plain-Python agent loop (no framework yet) with caps and a `stop_reason` span attribute; the workflow
  path for the simple types.
- **Verify:** run the golden set and look at the stop reasons. Did anything hit the cap, and why?

### D16 · Routing: cheap → escalate 🎤
- **Read:** 5.4.
- **Decide (ADR):** the routing strategy (classify-first, try-then-escalate or rules); the escalation triggers.
- **Build:** a router with a `route_reason` attribute, and per-route evals.
- **Verify:** the guide's demo line with *your* numbers: "cheap handles X% of traffic; escalation is Y% of traffic and
  Z% of spend", with quality per route from the evals.
- **🎤 Drill:** LLM system design ("design support automation for a marketplace").

### D17 · Port to LangGraph
- **Read:** 5.3.
- **Decide (ADR):** LangGraph vs Pydantic AI vs staying on the plain loop. Port one and compare against the plain
  loop you already have, on code size, testability and what you get for free.
- **Build:** the agent as a LangGraph state graph (nodes: triage → route → tools → draft → check).
- **Verify:** the golden set passes at the same rate as before the port. Draw the graph from memory.

## Phase 7 — Knowledge: RAG (Parts 6, 7, 8)

You've done hybrid RAG once (OSHA). New here: three languages and two scripts, exact identifiers in every ticket,
and a help center that's wrong in places.

### D18 · Embeddings for three languages
- **Read:** 6.1, 6.2.
- **Decide (ADR):** the embedding model (does it handle Armenian *and* translit?); the chunking strategy for help
  articles; whether HY/RU/EN versions of one article are one document or three.
- **Build:** a small embedding bake-off: 3 models × 40 cross-language query→article pairs. Include at least one free local multilingual model (e.g. an e5/bge-m3 class model). Check today whether OpenRouter serves embeddings; if not, local is the default.
- **Verify:** recall@5 per model per language. Pick with numbers.

### D19 · Ingestion, pgvector, HNSW
- **Read:** 6.3, 6.4, 6.5.
- **Decide (ADR):** incremental ingestion (hash per document); how deletions and updates propagate; HNSW parameters.
- **Build:** the ingestion pipeline and the pgvector table + index.
- **Verify:** the client sends an updated policy today (scripted event). Re-ingest, and prove the old version can't be
  retrieved any more.

### D20 · Keyword search + RRF 🎤
- **Read:** 7.1, 7.2.
- **Decide (ADR):** Postgres has no Armenian text-search config, so `simple` config, trigram, or a BM25 extension?
  How to match tracking-code formats exactly.
- **Build:** keyword retrieval, RRF fusion, both retrievers' results in the trace.
- **Verify:** queries with exact codes plus paraphrases: hybrid vs vector-only vs keyword-only, with numbers.
- **🎤 Drill:** decomposition case #2.

### D21 · Grounded drafts with citations
- **Read:** 8.1, 8.2.
- **Decide (ADR):** retrieval as a tool the agent calls vs a fixed workflow step; the citation format; the "not
  found" path; whether the draft quotes the policy.
- **Build:** a reply-draft node that returns a structured answer with chunk-ID citations; golden items extended with
  expected chunk IDs.
- **Verify:** retrieval and generation scored separately on the golden set.

### D22 · Reranking and query rewriting
- **Read:** 7.3, 8.3.
- **Decide (ADR):** add a reranker or not (only if the evals say it pays); query rewriting/translation for translit
  tickets; metadata filters (language, policy version).
- **Build:** whatever you decided, behind a config flag.
- **Verify:** quality gain vs latency and cost added. A "no" with numbers is a fine result.

### D23 · The RAG span tree + happy-path demo
- **Read:** 8.4, 10.4.
- **Decide:** the span names and attributes for the whole request (the guide's tree is the template).
- **Build:** the complete span tree: HTTP → agent.run → retrieval.hybrid → gen_ai.chat → tool.*.
- **Verify:** the guide's happy path. A ticket with an exact tracking code that vector-only gets wrong, hybrid gets
  right, and the span tree shows which retriever found it.

## Phase 8 — Evals, deep (Parts 9.5–9.7, 10.7)

### D24 · LLM-as-judge rubric 🎤
- **Read:** 9.5.
- **Decide (ADR):** what the judge grades (faithfulness to policy, tone, correct next action); binary vs 3-level;
  the judge model (different provider from the drafter).
- **Build:** a judge with reasoning-before-verdict and ground-truth evidence passed in.
- **Verify:** run it on the golden set, then read 20 verdicts. Do you agree?
- **🎤 Drill:** live debugging (Claude breaks something; you find it).

### D25 · Aligning the judge
- **Read:** 9.5 (alignment) again.
- **Do (you):** hand-label 100 drafts pass/fail. About 2 hours, and it's the most valuable 2 hours in the plan.
- **Build:** an agreement/precision/recall report.
- **Decide:** whether the judge is trustworthy enough to gate on. If not, change the rubric and re-run.
- **Verify:** the judge's precision and recall at catching failures, written into the ADR.

### D26 · RAG metrics, trajectory evals, evals → traces
- **Read:** 9.6, 9.7, 10.7.
- **Decide:** the expected tool calls per golden item; the offline vs online eval split (what share of live traffic
  gets sampled and scored).
- **Build:** context precision/recall, faithfulness and trajectory checks; scores written back to traces.
- **Verify:** "show me every trace where the judge failed" leads you straight to a failing conversation.

## Phase 9 — Human approval and durable runs (Parts 5.5–5.8)

### D27 · Human approval, durable runs
- **Read:** 5.6, 5.7.
- **Decide (ADR):** what needs approval (compensation always? above a threshold?); who approves; what the approval
  message says (what, to whom, how to undo); the checkpointer storage.
- **Build:** a LangGraph interrupt before `propose_compensation` is confirmed, a Postgres checkpointer, and an
  approve/reject endpoint.
- **Verify:** kill the process mid-run, restart it, and watch it resume from the checkpoint. A non-technical
  reviewer (Claude in character) understands the approval message at a glance.

### D28 · Background jobs 🎤
- **Read:** 5.8.
- **Decide (ADR):** ARQ vs Celery; idempotency keys; retry policy; how the trace ID crosses the queue.
- **Build:** a ticket-intake endpoint that enqueues the job, a worker that runs the graph, and status polling.
- **Verify:** retry a job twice and show no duplicate proposals. One trace spans HTTP → queue → worker.
- **🎤 Drill:** customer role-play ("why should we trust it?").

### D29 · Multi-agent: do we need it?
- **Read:** 5.5, 2.4 (context isolation).
- **Decide (ADR):** split into sub-agents (e.g. investigator + drafter) or not. Run the experiment, then decide.
- **Build:** a sub-agent variant behind a flag.
- **Verify:** quality, cost and latency vs the single agent. A "no" with numbers is the expected senior answer.

## Phase 10 — Security (Part 11)

### D30 · Prompt injection
- **Read:** 11.1, 11.2.
- **Decide (ADR):** map all ten OWASP risks to Arev and a mitigation each; the input screening approach.
- **Build:** delimiters + rules, an input screener, adversarial golden items. (Scripted event today: the client
  forwards something suspicious.)
- **Verify:** an injected ticket and an injected help article. The agent doesn't follow either, the guardrail span
  shows the block, and the eval case keeps it blocked.

### D31 · Output guardrails + budgets
- **Read:** 11.3, 11.4.
- **Decide (ADR):** PII redaction before model calls (phones, addresses) per the client's data rules; output checks;
  per-user and global budgets.
- **Build:** redaction, output validators, budget caps, a cost alert.
- **Verify:** a runaway loop gets stopped by the budget, and a PII test shows nothing raw leaves your boundary.

## Phase 11 — MCP (Part 12)

### D32 · MCP server 🎤
- **Read:** 12.1, 12.2, 12.3.
- **Decide (ADR):** which 2–3 tools to expose to Arev's ops team via MCP; stdio vs Streamable HTTP; read-only or not.
- **Build:** an MCP server (Python SDK, current spec revision, checked today) reusing the D13 tools.
- **Verify:** connect two different hosts with one config line each; both use the tools correctly.
- **🎤 Drill:** decomposition case #3.

### D33 · MCP auth + tool tuning
- **Read:** 12.3 (OAuth), 12.4.
- **Decide:** OAuth scopes per tool; versioning and publishing.
- **Build:** auth on the remote server; tool descriptions rewritten after watching which tool the host picks.
- **Verify:** an unauthorised scope gets refused. A2A, explained in one line.

## Phase 12 — Production hardening (Part 15, 10.8)

### D34 · Production hardening
- **Read:** Part 15.
- **Decide (ADR):** the fallback model chain; response caching rules; load-test targets.
- **Build:** provider failover recorded in traces, a response cache, a load test (Locust or k6) at 3× the client's
  peak hour.
- **Verify:** p50/p95/p99, error rate, cost at peak. Kill one provider mid-test and watch the failover.

### D35 · The debugging drill
- **Read:** 10.8.
- **Do:** Claude injects three faults on a branch without telling you (a chunk-size change, malformed tool JSON,
  hidden instructions in a document). Find and fix each from traces and logs only, in under 10 minutes, with a cost
  number for each incident.
- **Verify:** time per fault. Log all three in `incidents.md`.

## Phase 13 — The pilot in the client's world (Part 16.5–16.7)

### D36 · Integrating into their world 🎤
- **Read:** 16.5.
- **Do:** a role-play with the IT lead. Real constraints will come up.
- **Decide (ADR):** read replica vs webhooks; SSO; where data and model calls may go; deployment windows.
- **Build:** an architecture diagram *under their constraints*, plus any code changes the constraints force.
- **Verify:** the IT lead (in character) approves the diagram.
- **🎤 Drill:** design under enterprise constraints.

### D37 · The pilot
- **Read:** 16.4 again.
- **Do:** replay a "pilot week" of tickets (scripted event: it's not a normal week). Measure against the D03 success
  metric.
- **Decide:** go / extend / stop, with the numbers.
- **Verify:** a one-page pilot result: metric vs target, cost per month in AMD, failures and what you'd change.

### D38 · Demo to the COO
- **Read:** 16.6.
- **Do:** record a 5–7 min Loom, then do a live demo role-play with the sponsor. Show the "I don't know, here's the
  source" path.
- **Verify:** the guide's gate. The sponsor (in character) says back what it does and what it costs.

### D39 · Runbook + handoff 🎤
- **Read:** 16.7.
- **Build:** the runbook (every item in 16.7's list).
- **Do:** someone from the client team (in character) follows the runbook while you watch silently. Every place
  they get stuck is a doc bug; fix them.
- **🎤 Drill:** "what broke after launch?" Answer from your own `incidents.md`.

## Phase 14 — The model layer, optional (Parts 13, 14)

The guide says these are rarely required in postings. They're here for credibility: "we evaluated fine-tuning,
here's the delta, here's why we did or didn't ship it." GPU rental (~$20–40) doesn't fit inside the $20/month API cap: either treat it as a separate one-off budget, or train a small (1–3B) model on a free Kaggle/Colab GPU. That choice is the first decision on D42.

### D40 · Should we fine-tune?
- **Read:** 13.1, 13.2.
- **Decide (ADR):** is there a *form* problem the prompt can't pin down? (Candidate: triage on a small model, to cut
  the cheap tier's cost further.) Fine-tune, distill or neither?
- **Verify:** the decision, written using the Prompt → RAG → Fine-tune → Distill framework, with numbers from your evals.

### D41 · Training data from traces
- **Read:** 13.4.
- **Build:** mine traces, filter by the aligned judge, hand-correct, split train/val/test (the golden set stays held out).
- **Verify:** dataset stats, a PII scan, a dedupe report.

### D42 · QLoRA run
- **Read:** 13.3, 13.5.
- **Decide:** the base model, rank, epochs, GPU provider (cost shown first).
- **Build:** a tiny-subset local dry run, then a QLoRA run with Unsloth on a rented GPU.
- **Verify:** the run completes; you have an adapter and its training curves.

### D43 · Honest eval + serving with vLLM 🎤
- **Read:** 13.6, 13.7, 14.1, 14.2.
- **Build:** serve base + adapter with vLLM (OpenAI-compatible); plug it into the router as a model option.
- **Verify:** golden-set delta vs base, vs the cheap API, vs prompt+RAG, plus a general-capability sanity check.
- **🎤 Drill:** model-layer questions (when to fine-tune, LoRA, why self-hosting usually loses).

### D44 · Benchmark + the crossover verdict
- **Read:** 14.3.
- **Build:** a throughput benchmark at concurrency 1/16/64, TTFT with/without prefix caching, and quantization levels.
- **Verify:** the guide's chart (quality vs cost per 1M tokens) and the sentence "self-hosting beats the API only
  above N tickets/day", with your N.

## Phase 15 — Portfolio (Part 22)

### D45 · The case study
- **Read:** 22.1–22.3, 19.3.
- **Build:** a case-study doc: problem, architecture under constraints, what you cut, eval numbers, cost per ticket,
  routing split, known limitations, what broke. Plus 3–5 CV lines, each with a number you measured.
- **Verify:** someone who's never seen the project can explain it from the doc (Claude plays a recruiter, then an HM).

### D46 · Retro + final mock loop 🎤
- **Do:** re-read every ADR. Which decisions would you reverse, and what evidence came too late?
- **🎤 Drill:** a compressed full loop: screen → take-home walkthrough → case → behavioural, about 2.5 hours.
- **Verify:** scores vs your first drill on D05.
