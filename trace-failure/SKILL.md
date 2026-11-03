---
name: trace-failure
description: Attribute a failed end-to-end run to the first stage that failed, using the stage logs. Use when a run, a demo or a CI job fails and the cause must be found.
---

# Trace a failed run

A failed run is attributed to the first stage that failed (PRD, section 5).

## Steps

1. Get the records for the `run_id`, ordered by stage S1 to S7 and by time.
2. Find the first record with `status = fail`. That stage owns the failure. Later failures are side effects.
3. Read its `reason` and the record before it. Check whether the input was already weak, for example low confidence in S1 that still passed.
4. Classify the cause:
   - recognition (S1): missed control, wrong type, decoy accepted
   - mapping (S2): wrong device, verification pulse not seen
   - proposal or trigger (S3, S4): wrong intent, false trigger
   - channel (S5): command not delivered, CV check wrong
   - physical (S6): safety check cancelled the action, path or press failed
   - learning (S7): update not stored
5. Check whether the fallback worked. A failed wireless command followed by a successful physical press is a working fallback, not a failed run.

## Output

Three lines: run id and scenario, the owning stage with its cause class, and the one next check (a log line, a frame, or a unit test to run). Do not propose a fix before the cause is confirmed.
