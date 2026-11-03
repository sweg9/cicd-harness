# Common rules (every repo)

| Id | Rule |
|---|---|
| C-01 | **Traceability.** Every test, prompt file and pull request names the requirement it serves (`FR-xx`, `NFR-xx`). A requirement without a test is reported as a gap. Ids in the PRD are never renumbered; new ones are appended and old ones are marked deprecated. |
| C-02 | **One vocabulary.** Stages are `S1`..`S7`, levels `L0`..`L3`, channel types `IR`, `WIFI`, `BT`, `PHYS`, control kinds `switch`, `button`, `dial`, `remote`. See `data-model.md`. No other spelling in code, logs, data or docs. |
| C-03 | **Level gate.** Any function of level L2 or L3 checks the current level from the Capability Probe first. If a measurement falls under its threshold, the system drops one level at once. A function never runs above the current level. |
| C-04 | **Safety first.** No physical action unless the Safety Guard passed in the same run. A failed check cancels the action, notifies the user and is logged. At L3 or above, motion stops within 1 s of a stop command. High-risk devices (`risk_class = high`, for example heating) are never actuated by a proactive routine or by a physical press. |
| C-05 | **Privacy.** Raw video stays at the edge or below. Only crops and metadata leave the home. Frames are not stored by default, regions with people are masked, and proactive features are opt-in. No faces, no personal data and no home video in any repo or dataset without written consent. |
| C-06 | **User control.** Every proactive feature can be paused at once and switched off one by one. Manual control is never blocked by a failure of the robot or the cloud. |
| C-07 | **Explainability.** Every automatic action leaves an `ExecutionLog` entry with intent, confidence, channel and reason. The user can read why it happened. |
| C-08 | **Verify before trust.** A mapping becomes `confirmed` only after a verified state change (a test pulse seen by CV, or a CV-checked command). An unverified mapping is never used by a proactive routine. |
| C-09 | **No silent failure.** A run ends in exactly one of: success (verified), user notified of failure, or cancelled with a reason. The channel order is wireless by priority, then physical last. A failed channel is demoted, not retried forever. |
| C-10 | **No secrets.** No keys, tokens or passwords in a repo, a log, a prompt or a dataset. Use environment variables or a secret store. |
| C-11 | **Git hygiene.** Work on small commits with one purpose each and an imperative English subject of 72 characters or fewer. Generated files, build outputs and logs are ignored, not committed. |
| C-12 | **Targets are configuration.** Numeric targets (thresholds, rates, timeouts) are hypotheses. They live in one configuration file per repo and in the PRD, never as magic numbers in logic. |
