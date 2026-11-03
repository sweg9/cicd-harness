# harness

One place for the standards every repo must follow, the shared skills, and CI/CD. The other repos mount this repo as `skills/` at their root (git submodule). Requirements (FR, NFR) and metrics (E1 to E10) follow `sim-demo/PRD.md` in [docs](https://github.com/sweg9/docs), which is also published as the [wiki](https://sweg9.github.io/docs/).

## Contents

| Folder | Kind | What it does |
|---|---|---|
| `standards/` | Standards | Mandatory rules for every repo: common (C-01 to C-12), AI output (A-01 to A-08), logs (L-01 to L-08), data model, per-repo obligations |
| `check-conformance/` | Skill | Checks code, prompts and data against the standards, with file and line evidence |
| `check-execution-log/` | Skill | Validates stage-record logs (JSON Lines) against the standard |
| `validate-dataset/` | Skill | Checks the label schema, privacy and the train/evaluation split |
| `check-fr-coverage/` | Skill | Checks that each of FR-01 to FR-16 has a test |
| `run-stage-eval/` | Skill | Computes metrics E1 to E10 from logs and compares them with the targets |
| `trace-failure/` | Skill | Attributes a failed run to the first failed stage and classifies the cause |
| `prompt-regression/` | Skill | Compares a new prompt version with the previous one on a fixed evaluation set |
| `build-sim-demo/` | Skill | Builds sim-demo for macOS and Windows and packages the zip files |
| `release-gate/` | Skill | Runs the checks above in order and returns go or no-go |

## Using it from another repo

`skills/` at the root of each repo points to a specific commit of this repo, and `.claude/skills` links to `../skills`, so Claude Code sees the skills as soon as it opens the repo.

```bash
git clone --recurse-submodules <repo url>
git submodule update --init            # in a repo that is already cloned
git submodule update --remote skills   # move to the latest skills, then commit
```

Standards and skills are changed only in this repo, never inside `skills/` of another repo. The docs wiki copies them at build time.

## Language

All documents and text in every repo are written in English. Code comments may be in Korean.
