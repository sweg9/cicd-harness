# Per-repo obligations

Each repo owns the items in its row and must satisfy the rules named there. "Gap" marks something known to be missing today.

| Repo | Owns | Must satisfy |
|---|---|---|
| `dataset` | Labeled data and the frozen evaluation sets | Label schema below; C-05 and C-10 for every file; splits that do not leak; an evaluation set is frozen and versioned, failed cases are added as a new version |
| `ai-engine` | Models and prompts for S1, S2, S4, S5, S6 | A-01 to A-08; C-03; L-08 stage records for its stages; metrics E5 to E8 reproducible with the harness |
| `backend-server` | Mapping Registry, Routine Engine, Control Orchestrator, Safety Guard, logs, Redis | `data-model.md` entities; L-01 to L-08; C-03 level gate; C-04 Safety Guard decides before any physical command; durable store for mappings and logs, so Redis persistence (AOF) is on; C-06 pause and off switches |
| `sim-demo` | The Unity simulator demo | Shows S1 to S7 with these ids; behavior rules stay equal to PRD section 6; builds only through `build-sim-demo`; C-06 keys documented in the README. **Gap:** it shows its log on screen only and does not write stage records yet |
| `live-demo` | The demo with a real camera and AI | C-05 (crop-only upload, opt-in), C-04 (visible stop control), C-03 (Capability Probe before the first run), L-01 to L-08 |
| `docs` | PRD, plan, prompts, this team's decisions | Requirement ids never renumbered (C-01); targets marked as hypotheses; every standards change explained in its commit |
| `harness` | Skills, standards, CI | Every merge into any repo passes the gate in `release-gate`; standards change only here |

## Label schema (dataset)

One JSON object per labeled control, in a JSON Lines file per image set:

| Field | Rule |
|---|---|
| `image_id` | Opaque id of a crop or frame stored outside the repo or already masked |
| `kind` | actuator kind enum |
| `bbox` | `[x, y, w, h]`, each 0 to 1 |
| `device_id` | Optional, the mapped device when known |
| `room`, `session` | Used for splitting: one room or session never appears in both train and evaluation |
| `occluded`, `decoy` | Booleans. A decoy looks like a control but is not one (for example a thermostat) |
| `consent` | Reference to the consent record |

## Pull request check

Before merging in any repo: the `release-gate` skill returns go. A pull request description lists the requirement ids it touches (C-01).
