# Data model and shared enumerations

Source: design paper, section "Architecture and Data Model". A **Mapping** links an **Actuator** to a **Device**; a Device has one or more **ControlChannel**s; every execution leaves an **ExecutionLog** entry.

## Entities

| Entity | Fields |
|---|---|
| Actuator | `actuator_id`, `kind`, `pose`, `embedding`, `affordance` |
| Mapping | `mapping_id`, `status`, `confidence`, `verified_at` |
| Device | `device_id`, `name`, `type`, `room`, `risk_class` |
| Routine | `routine_id`, `trigger`, `condition`, `action`, `autonomy_level` |
| ControlChannel | `channel_id`, `type`, `params`, `success_rate`, `latency` |
| ExecutionLog | `log_id`, `channel`, `outcome`, `verify_result`, `reason`, `ts` |

Cardinality: Actuator 1..* Mapping, Mapping *..1 Device, Device *..* Routine, Device 1..* ControlChannel, Routine 1..* ExecutionLog, ControlChannel 1..* ExecutionLog.

## Enumerations (exact spelling)

| Name | Values |
|---|---|
| stage | `S1` `S2` `S3` `S4` `S5` `S6` `S7` |
| level | `L0` `L1` `L2` `L3` |
| actuator `kind` | `switch` `button` `dial` `remote` |
| channel `type` | `IR` `WIFI` `BT` `PHYS` |
| mapping `status` | `candidate` `verified` `confirmed` `rejected` |
| `risk_class` | `low` `medium` `high` |
| `autonomy_level` | `suggest` `ask` `auto` |
| `outcome` | `success` `failed` `cancelled` |
| `verify_result` | `changed` `unchanged` `uncertain` `not_checked` |

## Rules

- Identifiers are opaque strings, unique within their entity, and never reused.
- `confidence` and `success_rate` are numbers in 0 to 1. `latency` is in milliseconds. `ts` is UTC, ISO 8601.
- Positions in images are normalized to 0..1 of the image size. Positions in the home are in meters.
- A `confirmed` mapping needs `verified_at`. Only `confirmed` mappings may be used by a proactive routine (C-08).
- Mappings and logs are durable (FR-05, FR-16). A cache may hold copies; it is never the only copy.
