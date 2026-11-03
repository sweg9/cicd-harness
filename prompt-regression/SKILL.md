---
name: prompt-regression
description: Run a new prompt version against the fixed photo set and compare it with the previous version before it is adopted. Use when a prompt file under prompts/ changes or a new version is proposed.
---

# Prompt regression

Prompt ids, rules and templates: https://sweg9.github.io/docs/product/prompts. Output rules: `standards/ai-output.md`.

## Steps

1. Identify the prompt id and the two versions: current and proposed.

| Prompt | Requirement | Measure |
|---|---|---|
| P-S1 Read a control | FR-01 | Kind and state accuracy; share marked not clear |
| P-S2 Check a mapping | FR-02 | Agreement with labeled before and after pairs |
| P-S4 Infer intent | FR-05 | Intent and device accuracy; answers within about 8 s |
| P-S5 Verify a result | FR-08 | Yes or no accuracy against labeled outcomes; share of `unsure` |

2. Check the rules first: header with model, date, reason and results; JSON output with `confidence` and `reason`; `unsure` allowed; up to 3 crops. If a rule is broken, stop and report it.
3. Run both versions on the same photo set with the same model and settings. Do not change the set between versions.
4. Count JSON parse failures, time-outs and `unsure` answers for each version.
5. Accept the new version only if its measure is not worse than the old one.

## Output

Old and new values side by side, the verdict (accept, reject, or needs a larger set), and the failed cases to add to the photo set.
