---
name: mistakes
description: Review and log Saro's mistakes in Arev Desk. Use when he says /mistakes, asks to review or understand a mistake, asks "why was that wrong", or when a check answer, decision or role-play move in a session was wrong or weak and should be recorded.
---

# Mistakes

The log is `mistakes.md` at the repo root. It records wrong moves (not just wrong facts) so Saro can review them
and stop repeating them. Follow the teaching rules in `AGENTS.md`: teach, don't decide for him.

## Two modes

### Log (during or at the end of a session)

When Saro gets a check question wrong, makes a weak decision, steers the client in a role-play, or skips something a
strong FDE would have asked:

1. Add a row to the table at the top of `mistakes.md` (next number, day, `open`, category, short description).
2. Add a section below it with exactly these fields: **What I did**, **What was wrong**, **Right move**,
   **Why I did it (to review)**, **Next time**.
3. Leave "Why I did it" as a guess marked to review. The real reason is his to say in review mode.
4. Only log real mistakes. Don't pad the log with things he did well or with style nitpicks. One line to him saying
   what you logged is enough.

### Review (when he asks, or at session start if any entry is `open`)

For each `open` entry, one at a time:

1. Show the entry's "What I did" and ask him: *why did you do that?* Wait for his answer. Don't explain first.
2. Say what is exactly wrong, in 3–5 sentences, in plain words. Point to the guide section if there is one.
3. Check the pattern, not the instance: ask him to name the rule he would apply next time. If his rule is
   vague, tighten it with him.
4. Give a fresh, small scenario (a different client or ticket than the original) and ask what he would do. If it is
   right, mark the entry `understood`. If not, explain again from a different angle and stay `open`.
5. Update the entry: his real reason under "Why I did it", his rule under "Next time", and the status.

Mark an entry `fixed` only after he does the right thing in a later session. Note the day.

## Rules

- Keep explanations short. He asked for no over-explaining.
- Don't shame, don't soften. State what was wrong plainly.
- If a category repeats (3+ entries), say so and name the underlying habit.
- Never reveal or quote anything from `engagement/.gm/` while reviewing.
