# Interview drills (🎤 days)

45–60 min each, with a timer. Claude plays the interviewer and stays strict. Log the score in the session file.

| Day | Drill | Guide | Format |
|---|---|---|---|
| D05 | Decomposition case #1 | 16.8 | 45 min. "A pharmacy chain wants AI to cut prescription-refill calls" |
| D09 | Eval design | 9 | 30 min. "How do you know your classifier works?" |
| D12 | Take-home walkthrough | 16.3, 17 | 30 min. Defend D05–D12; name what you cut |
| D16 | LLM system design | 6–8, 5.4 | 45 min. "Design support automation for a marketplace" |
| D20 | Decomposition case #2 | 16.8 | 45 min. "An insurer wants AI in claims intake" |
| D24 | Live debugging | 10, 5.1 | 30 min. Claude breaks the agent; find it from traces |
| D28 | Customer role-play | 16.9 | 30 min. Sceptical VP: "why should we trust it?" |
| D32 | Decomposition case #3 | 16.8 | 60 min, messy-dataset variant: prototype + pitch |
| D36 | Enterprise constraints | 16.5 | 30 min. SSO, residency, change control |
| D39 | "What broke after launch?" | 16.9 | 30 min. From your own incidents.md |
| D43 | Model-layer questions | 13.1, 14.1 | 30 min. Fine-tune or not, LoRA, self-host crossover |
| D46 | Compressed full loop | 17 | ~2.5 h. Screen → walkthrough → case → behavioural |

## Rubric for decomposition cases (score 1–4 each)
1. **Clarified before solving:** volume, data, users, constraints, success.
2. **Carved cleanly:** non-overlapping subproblems.
3. **Stated assumptions out loud** and kept moving.
4. **Named failure modes** and how each is caught (evals, traces, HITL).
5. **Compared two approaches** on cost, latency, complexity, maintenance.
6. **Picked an MVP,** said what was cut, said how the pilot is measured.
7. **Narrated throughout,** with no long silences.

Track the total over time: D05 is the baseline, D46 the result.
