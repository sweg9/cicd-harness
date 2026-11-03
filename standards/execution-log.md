# Execution log

Every component writes one **stage record** for each stage it takes part in. This is how a run is traced from S1 to S7 (FR-16, NFR-08) and what the harness reads to compute the metrics (E1 to E10).

## Stage record (required fields)

| Field | Type | Rule |
|---|---|---|
| `run_id` | string | Same for every record of one run |
| `scenario` | `1` `2` `3` or `null` | The demo scenario, `null` outside evaluation |
| `stage` | stage enum | `S1`..`S7` |
| `component` | string | Who wrote it, for example `ai-engine`, `backend-server`, `sim-demo` |
| `status` | `ok` `fail` `skipped` | `skipped` needs a reason |
| `ts` | UTC ISO 8601 | Start of the stage |
| `latency_ms` | integer | Duration of the stage |
| `reason` | string | One sentence. Required when `status` is `fail` or `skipped` |

Optional but expected when they apply: `actuator_id`, `device_id`, `channel` (channel type), `confidence`, `verify_result`, `level`.

An `ExecutionLog` entity (see `data-model.md`) is written in addition for every action that changes a device state (S5, S6).

## Rules

- L-01 One record per stage per run per component. Retries are separate records with the same `run_id` and increasing `ts`.
- L-02 Records are append-only. A record is never edited or deleted.
- L-03 A record is written even when the stage fails or is cancelled.
- L-04 No S6 `ok` record without an earlier Safety Guard `ok` record in the same run (C-04).
- L-05 S6 follows a failed or missing S5 channel. A run that goes to S6 while S5 succeeded is a violation.
- L-06 A failed run is attributed to the first record with `status = fail` in stage order (see the `trace-failure` skill).
- L-07 Logs carry no raw images, no faces and no secrets (C-05, C-10). They may name a crop by id.
- L-08 Format: JSON Lines, one record per line, UTF-8.

## Example

```json
{"run_id":"r-0007","scenario":2,"stage":"S5","component":"backend-server","status":"fail","ts":"2026-11-03T06:41:12Z","latency_ms":2100,"channel":"WIFI","verify_result":"unchanged","reason":"Plug accepted the command but the lamp did not change."}
{"run_id":"r-0007","scenario":2,"stage":"S6","component":"backend-server","status":"ok","ts":"2026-11-03T06:41:19Z","latency_ms":5400,"channel":"PHYS","verify_result":"changed","reason":"Switch pressed after safety check."}
```
