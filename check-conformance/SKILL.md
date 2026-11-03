---
name: check-conformance
description: Check a repo or a pull request against the mandatory project standards (rules C-01 to C-12, A-01 to A-08, L-01 to L-08) and report violations with file and line evidence. Use before merging, when reviewing code, prompts or data, or when asked whether something follows the standards.
---

# Check conformance

Rules: the `standards` skill (`standards/common.md`, `ai-output.md`, `execution-log.md`, `per-repo.md`). Read the repo's row in `per-repo.md` first.

## Steps

1. Identify the repo and the scope (whole repo, or the diff of a pull request).
2. Search for evidence of each rule that applies to this repo. Report a violation only with a file and line. Do not infer from names.

| Rule | What to look for |
|---|---|
| C-02 | Other spellings of stages, levels, channel types or control kinds (for example `wifi`, `phys`, `Stage 5`, `toggle`) in code, logs, data, docs |
| C-03 | Calls to wireless or physical control that do not check the level first |
| C-04 | A path to a physical command that does not pass the Safety Guard; actuation of a `high` risk device; missing cancel, notify or log on a failed check |
| C-05 | Upload or storage of full frames or video; faces or people in data; missing masking; proactive features on by default |
| C-06 | Missing pause or off switch for a proactive feature; manual control that depends on the robot or cloud |
| C-07, L-01..L-08 | Automatic actions without an `ExecutionLog` entry; stage records missing fields or edited after writing |
| C-08 | A proactive routine that uses a mapping which is not `confirmed` |
| C-09 | A run that can end without success, notification or cancel; endless retry of a failed channel |
| C-10 | Keys, tokens, passwords, personal data (search the diff and the history of the scope) |
| C-11 | Generated files, build outputs or logs committed |
| C-12 | Numeric thresholds written inline in logic |
| A-01..A-08 | Model output used without schema validation; missing `confidence` or `reason`; prompt files edited in place; prompts with personal data |

3. For each rule that cannot be checked from the code (for example "the user can read why"), write "not checkable here" and name who must confirm.

## Output

A table of rule id, status (`pass`, `violation`, `not checkable`), and evidence (`file:line`). Violations of C-04, C-05 and C-10 come first and are blockers. End with the count per status. Do not fix anything unless asked.
