---
name: release-gate
description: Decide go or no-go for merging or releasing a change in any project repo by running the conformance, log, coverage, evaluation and regression checks in order. Use before merging a pull request, before a demo or when asked if something is ready.
---

# Release gate

Combines the other skills of this repo. Targets and requirement priorities come from the PRD in https://github.com/sweg9/docs (`sim-demo/PRD.md`).

## Steps

Run in this order and record each result before the next step.

1. `check-conformance` on the diff. **Blocker:** any violation of C-04, C-05 or C-10.
2. `validate-dataset` if data changed. **Blocker:** "not ready".
3. `check-fr-coverage`. **Blocker:** a "must" (M) requirement touched by the change with no test.
4. `check-execution-log` on the latest evaluation logs, then `run-stage-eval`. **Blocker:** a stage below 80 % (E2), or any metric that gets worse than the last accepted run by more than the allowed drop in configuration. Metrics missing for lack of data are "not measured", not passed.
5. `prompt-regression` if a prompt file changed. **Blocker:** verdict "reject".
6. `build-sim-demo` if `sim-demo` changed. **Blocker:** the build fails.

## Verdict

- **go**: no blocker and nothing "not measured" among the checks the change needs.
- **go with notes**: no blocker, but some checks are "not measured" or "low sample". List them.
- **no-go**: at least one blocker.

## Output

A table of step, result and blocker, then the verdict in one line and the shortest list of what must change. Do not merge, tag or publish anything. Say what was not run and why.
