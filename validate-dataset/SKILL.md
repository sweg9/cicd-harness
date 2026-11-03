---
name: validate-dataset
description: Validate the switch-panel labels against the labeling spec and the privacy and split rules before training or evaluation. Use when labels are added or changed in the dataset repo, or before a test set is frozen.
---

# Validate dataset

Label schema: `standards/per-repo.md` and https://sweg9.github.io/docs/dataset/labeling. Enumerations: `standards/data-model.md`. Privacy: rule C-05.

## Steps

1. Read the label files (one JSON record per image). Count records that fail to parse.
2. Schema: every record has `image_id`, `consent`, `room_id`, `room_type`, `session`, `controls`, `status`. Every control has `kind` from the five kinds, `bbox` and `press_point` with values from 0 to 1 inside the image, and `state` from `on` `off` `unknown`.
3. References: every `image_id` resolves to a photo in S3, and every `consent` points to a consent record.
4. Privacy: no faces or people visible in a sample of at least 20 photos (all if fewer). No photos or video committed to git.
5. Split: one whole `session` is kept aside for testing, and no session appears in both sets. Exact duplicate photos across sets count as leakage.
6. Balance: counts per `kind`, per `room_type`, and the share of `decoy` and `occluded`. Report any kind under 5% of the set.
7. Evidence: `evidence` fields are present only for labeling checks and are excluded from model inputs.
8. Frozen sets: a frozen test set has a version and a content hash in its manifest. If a file changed and the hash did not, report it.

## Output

A table of check, status and count, then the first five offending records per failed check. State "ready", "ready with fixes" or "not ready". Do not edit labels or move files unless asked.
