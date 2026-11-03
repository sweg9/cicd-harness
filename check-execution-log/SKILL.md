---
name: check-execution-log
description: Validate stage-record logs (JSON Lines) against the execution-log standard before they are used for evaluation or tracing. Use when a log file is produced, before run-stage-eval or trace-failure, or when metrics look wrong.
---

# Check execution log

Schema and rules L-01 to L-08: `standards/execution-log.md`. Enumerations: `standards/data-model.md`.

## Steps

1. Parse the file as JSON Lines. Count lines that fail to parse.
2. Per record, check: required fields present and typed (`run_id`, `scenario`, `stage`, `component`, `status`, `ts`, `latency_ms`, `reason`), enum values spelled exactly, `ts` is UTC ISO 8601, `latency_ms` is a non-negative integer, `reason` is non-empty when `status` is `fail` or `skipped`.
3. Per run (`run_id`), check:
   - stages appear in order by `ts`, and a stage is not repeated unless it is a retry with a later `ts` (L-01)
   - S6 `ok` has an earlier Safety Guard `ok` record in the same run (L-04)
   - S6 appears only after an S5 `fail` or a missing wireless channel (L-05)
   - a failed stage has a record (L-03); a run with no S7 record after a verified action is incomplete
4. Check records for forbidden content: image data, base64 blobs, faces, secrets (L-07).
5. Compare record counts per component with what the run should have produced and list the missing ones.

## Output

Total records and runs, then a table of violation, rule id, count, and up to three examples with `run_id` and line number. State clearly whether the log is fit for metrics: "fit", "fit with exclusions" (list the excluded runs) or "not fit". Never repair the log silently.
