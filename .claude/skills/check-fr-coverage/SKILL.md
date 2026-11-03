---
name: check-fr-coverage
description: Check that every functional requirement (FR-01 to FR-16) has at least one test, and report the gaps. Use before a release or when asked about test coverage against the requirements.
---

# Check FR coverage

Requirements and their verification methods: https://github.com/sweg9/docs, `sim-demo/PRD.md`, section 3.

## Steps

1. List FR-01 to FR-16 with the verification method from the PRD (for example "Hazard-injection test" for FR-11).
2. In the repos under test, find tests that name the FR id in a test name, a marker or a comment. Search for `FR-` ids, do not guess from file names.
3. Mark each requirement as covered, partly covered (test exists but does not cover the stated verification) or missing.
4. For FR-10 and FR-12, note that they apply from level L3 only. A missing physical-robot test is acceptable when the capability probe reports a lower level, and the report says so.

## Output

A table of FR id, priority, test location and status. After it, the missing "M" (must) requirements first, then the "S" ones. Do not write tests unless asked.
