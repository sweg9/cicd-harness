---
name: validate-dataset
description: Validate a labeled dataset or evaluation set against the label schema and the privacy and split rules before training or evaluation. Use when data is added or changed in the dataset repo, or before an evaluation set is frozen.
---

# Validate dataset

Label schema: `standards/per-repo.md` ("Label schema"). Enumerations: `standards/data-model.md`. Privacy: rule C-05.

## Steps

1. Read the label files (JSON Lines). Count lines that fail to parse.
2. Schema: every record has `image_id`, `kind`, `bbox`, `room`, `session`, `occluded`, `decoy`, `consent`. `kind` is in the enum. Every `bbox` value is within 0 to 1, with positive `w` and `h`, and the box stays inside the image.
3. References: every `image_id` resolves to a file, and every file has at least one record or is explicitly marked as a negative. Every `consent` points to an existing consent record.
4. Privacy: no faces or people visible in the sample images; spot-check at least 20 images or all of them if fewer. Search the repo for raw video files and for personal data in file names.
5. Splits: no `room` or `session` appears in both the training and the evaluation set. List overlaps. Exact duplicate images across splits count as leakage.
6. Balance: counts per `kind`, per `room`, and the share of `decoy` and `occluded`. Report any kind under 5 % of the set.
7. Frozen sets: a frozen evaluation set has a version and a content hash in its manifest. If a file changed and the hash did not, report it.

## Output

A table of check, status and count, then the first five offending records per failed check with file and line. State "ready", "ready with fixes" or "not ready", and say what blocks training or evaluation. Do not edit labels or move files unless asked.
