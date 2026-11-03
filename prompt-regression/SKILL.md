---
name: prompt-regression
description: Run a new prompt version against the fixed evaluation set and compare it with the previous version before it is adopted. Use when a prompt file under prompts/ changes or a new version is proposed.
---

# Prompt regression

Prompt IDs, rules and targets: https://github.com/sweg9/docs, `sim-demo/PROMPTS.md`.

## Steps

1. Identify the prompt id (P-S1, P-S2, P-S4, P-S5, P-S6) and the two versions: the current one and the proposed one.
2. Check the rules first. The file header has model, date, reason and results. The output is JSON with `confidence` and `reason`, and it can return "uncertain". If a rule is broken, stop and report it.
3. Run both versions on the same evaluation set with the same model and settings. Do not change the set between versions.
4. Compute the matching metric: P-S1 precision and recall (E5), P-S2 Top-1 (E6), P-S4 false-trigger rate (E9), P-S5 state-check accuracy (E7), P-S6 press success (E8).
5. Count JSON parse failures and "uncertain" answers for each version.
6. A new version is accepted only when its metric is not worse than the old one and it meets the target in PROMPTS.md, section 4.

## Output

Old and new values side by side, the verdict (accept, reject, or needs a larger set), and the failed cases that should be added to the evaluation set.
