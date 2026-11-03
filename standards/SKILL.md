---
name: standards
description: The mandatory standards every project repo (dataset, ai-engine, backend-server, sim-demo, live-demo, docs, cicd-harness) must follow - shared vocabulary, safety, privacy, logging, AI output and per-repo obligations. Read it before writing, reviewing or changing code, prompts, data or docs in any of these repos.
---

# Project standards

These rules are mandatory. They come from the Phase 2 design (https://github.com/sweg9/docs, `sim-demo/PRD.md`) and make the seven repos work as one system. A rule has an id so that reviews, tests and the harness can cite it.

| File | Content |
|---|---|
| [common.md](common.md) | Rules C-01 to C-12 for every repo |
| [data-model.md](data-model.md) | Entities, identifiers and the shared enumerations |
| [execution-log.md](execution-log.md) | The stage record every component must write (FR-16, NFR-08) |
| [ai-output.md](ai-output.md) | Rules A-01 to A-08 for anything a model produces |
| [per-repo.md](per-repo.md) | What each repo owns and must satisfy, with known gaps |

## How to apply

1. Find the repo you are in and read its section in `per-repo.md`, then `common.md`.
2. Use the enumerations in `data-model.md` exactly as spelled. Do not add synonyms.
3. When a change breaks a rule, do not work around it. Change the standard first, in this repo, in its own commit, and say why.
4. Cite rule ids in review comments, for example "violates C-04".
5. Safety (C-04) and privacy (C-05) have no exceptions and no "later".

The numeric targets in the PRD are hypotheses and live in configuration, not in code (C-12). Everything in this folder is a rule, not a hypothesis.
