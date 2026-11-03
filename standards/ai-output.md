# AI output rules

Applies to every prompt and model in `ai-engine` and to every caller of it. Prompt ids and templates: `sim-demo/PROMPTS.md` in the docs repo.

| Id | Rule |
|---|---|
| A-01 | Output is JSON that matches a schema stored next to the prompt. Text around the JSON is rejected. |
| A-02 | Every result has `confidence` (0 to 1) and `reason` (one sentence). |
| A-03 | The model may answer `"uncertain"`. The caller then asks the user or falls back (C-09). It never guesses. |
| A-04 | Invalid output is rejected by code and retried once. A second failure is a stage failure with a reason. |
| A-05 | A prompt is a versioned file. A used file is never edited; a change is a new version with model, date, reason and evaluation result in its header. |
| A-06 | A new version is adopted only after the `prompt-regression` skill shows it is not worse than the old one on the fixed evaluation set. |
| A-07 | Inputs to a cloud model are crops and metadata only (C-05). No names, faces or personal identifiers in a prompt. |
| A-08 | Plans for physical actions return `{"abort":true}` when a person or an obstacle is on the path. The Safety Guard still checks independently (C-04). |

Confidence is not a decision. Thresholds (for example demoting a channel below 0.40) are configuration (C-12).
