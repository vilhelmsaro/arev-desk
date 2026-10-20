# Arev Desk — an FDE practice engagement

A made-up but realistic client engagement, built to work through the
**AI Automation & FDE Learning Guide** by making real decisions on a real-shaped system.

- **Client:** Arev Express, a last-mile courier in Yerevan (fictional).
- **Their problem, as they first state it:** "We want an AI chatbot." (Discovery will find out what they actually need.)
- **What you'll end up shipping:** whatever discovery and scoping justify. Most likely a support-desk copilot: ticket triage, grounded reply drafts, order and tracking tools, refunds behind human approval, an MCP server, evals, tracing and a pilot.
- **Stack:** Python 3.12 · uv · FastAPI · Postgres + pgvector · Docker Compose · LangGraph · Pydantic. Models from at least three providers.
- **Guide:** `~/Sync/obsidian/obsidian_vault/TASKS_SYSTEM/Projects/exercise-fde/ai-automation-fde-learning-guide.md`

## How it works

You make the decisions, Claude builds them, and you check the numbers.
Each day is one **session** of 2–5 hours:

1. **Read** the guide sections for the day (15–30 min). Claude asks you 2–3 questions to check you understood them.
2. **Decide.** Claude gives you a decision brief with 2–3 real options. You pick one and say why *before* Claude gives its own view, then defend your pick or change it. It gets recorded in `decisions/`.
3. **Build.** Claude Code implements your decision. You review the diff at the level of "is this what I decided?"
4. **Verify.** You run the day's check yourself and read the numbers. "It seems to work" doesn't count.
5. **Log** it in `sessions/` (10 min): what you decided, what surprised you, and anything that broke (→ `incidents.md`).

Every day has a **Core** part (about 2 hours, the minimum that keeps the plan intact) and a **Stretch** part for the days you have 4–5 hours.

At some points Claude plays the client: stakeholders, data drops, scope changes, a pilot that goes sideways. Don't read `engagement/.gm/`. It holds the client's hidden facts, and finding them out is the exercise.

## Starting a session

Open this folder in Claude Code and say: **"start today's session"**.
Claude reads `PROGRESS.md`, finds the next day in `PLAN.md`, and runs the session using `AGENTS.md`.

## Files

| File | What it is |
|---|---|
| `PLAN.md` | The 46-day plan, in order, mapped to the guide's parts |
| `PROGRESS.md` | Checklist, hours log, current day |
| `AGENTS.md` / `CLAUDE.md` | How Claude runs sessions and plays the client |
| `engagement/` | Client brief, discovery doc, scope doc (you fill these in) |
| `decisions/` | One ADR per decision, plus an index |
| `sessions/` | One log per day |
| `incidents.md` | Everything that broke, which later becomes your "what broke after launch?" answers |
| `rehearsals/` | Interview drills and how they're scored |
