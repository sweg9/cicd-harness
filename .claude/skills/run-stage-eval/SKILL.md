---
name: run-stage-eval
description: Compute the project evaluation metrics (E1-E10) per scenario and per stage from run logs and compare them with the targets. Use when asked for evaluation results, a metrics report or a pass/fail check against the PRD targets.
---

# Run stage evaluation

Targets come from the PRD in https://github.com/sweg9/docs (`sim-demo/PRD.md`, section 5).

## Input

Run logs written under FR-16, one record per stage. Proposed fields, to be aligned with backend-server:

`run_id, scenario (1|2|3), stage (S1..S7), status (ok|fail|skipped), latency_ms, reason`

If the logs have different field names, map them and say how in the report.

## Steps

1. Group records by `run_id` and `scenario`. A run succeeds when every stage it is expected to reach has `status = ok`.
2. E1: end-to-end success rate per scenario. Targets 90 / 80 / 70 %.
3. E2: success rate of each stage. No stage below 80 %.
4. E3: latency per stage and trigger-to-verified total. Scenario 1 total is 5 s or less.
5. E4: share of runs that reach S6 and the causes. Report only.
6. E5-E8: use the labeled sets for S1 (precision 0.90 / recall 0.85), S2 (Top-1 80 %), S5 (95 %) and S6 (90 %). If a set is missing, write "not measured" and do not estimate.
7. E9-E10: proposal acceptance (70 %), false trigger rate (under 10 %), registration time against manual.

## Output

One table with metric, value, target and pass/fail, then at most three lines on the worst stage. Include the number of runs behind each value. A rate from fewer than 10 runs is marked "low sample".
