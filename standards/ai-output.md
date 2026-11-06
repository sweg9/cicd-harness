# AI output rules

Applies to every prompt and model in `ai-engine` and to every caller of it. Prompt ids and templates: https://sweg9.github.io/docs/product/prompts (P-S1, P-S2, P-S4, P-S5).

| Id | Rule |
|---|---|
| A-01 | Output is JSON that matches a schema stored next to the prompt. Text around the JSON is rejected. |
| A-02 | Every result has `confidence` (0 to 1) and `reason` (one sentence). |
| A-03 | The model may answer `unsure`. The caller then follows C-09. It never guesses. |
| A-04 | Invalid output is rejected by code and retried once. A second failure fails the stage with a reason. |
| A-05 | A prompt is a versioned file. A used file is never edited; a change is a new version with model, date, reason and evaluation result in its header. |
| A-06 | A new version is adopted only after the `prompt-regression` skill shows it is not worse than the old one on the fixed photo set. |
| A-07 | Inputs to the cloud VLM are crops (up to 3) and short notes only (C-05). No names, faces or personal identifiers. |
| A-08 | A model answer never overrides the autonomy policy (C-03) or the safety gate (C-04). A time-out of the intent call (about 8 s) means the system only suggests. |

Confidence is not a decision. Thresholds and time limits are settings (C-12).
