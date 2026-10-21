# Verification

## Repository preparation

Fresh checks passed: queue patch replay, Agents patch application to 0.162.1, the isolated launcher fixture, and local documentation links. The host-specific shell wrapper is excluded.

## Queue

The first six patches are based on source commit `c93b64f8` on Codex 0.163.0. The public version uses `--help` in the launcher argument-forwarding fixture; production code is unchanged. Archive replay produced tree `d3e1ce36061c4c94862899fd514f2be50c8c3c94`.

The implementation session recorded:

- 464 focused TUI queue, Tab, and reconnect tests; 70 queue/goal extension tests; 24 app-server integration tests: all passed after correcting three outdated test expectations.
- Launcher install, argument forwarding, fallback, and rollback fixture: passed.
- Isolated TUI checks for independent edits, named destinations, triggers, actions, and both reorder key pairs: passed.
- Cross-server recovery regression: passed.
- Full-duration scenario with two goals and multiple threads: correct routing, no duplicate prompts, both goals complete, queue empty. The monitor initially failed on a model-picker assumption; picker interaction was verified separately.

The queue implementation’s CLI and package built successfully. Earlier packages passed disposable daemon install/restore checks; that check and the long scenario were not repeated after the final focused changes. Full workspace suites and other upstream releases are not covered by these results.

## Ephemeral side-thread fix

Patch 8 (`512b86a8`) prevents server queue list/claim/add calls for ephemeral `/btw` and `/side` conversations and preserves local Tab queuing and delivery. The old enhanced package reproduced the queue errors and rejected side-thread Tab submissions.

The focused regression and all 42 queue/side TUI tests passed. The rebuilt `0.163.0+btwfix.20261018` package passed repeated isolated terminal runs covering `/btw` startup, two side conversations per run, four queued side prompts delivered exactly once, returning to the parent, and persistent parent queuing. A further run covered switching to the parent and back while side work remained active. The persistent queue also retained separate entries through interruption and individual editing. Tests used a local mock Responses provider. Formatting and whitespace checks passed; full workspace suites were not rerun.

## Queue recovery deadlock fix

Patch 9 releases the thread-list permit and goal-resume guard before resumed-thread idle work dispatches a queued follow-up to another unloaded thread. Its regression stages both threads unloaded, completes the source goal, and checks that the follow-up runs and thread naming remains responsive.

The implementation logs record the regression failing without the fix and passing in three separate fixed runs. The related run had 119 of 121 tests pass (one flaky); two tests failed, so this is not a clean suite result. The CLI and package built successfully. These historical results were reused when preparing this patch; Rust tests and full workspace suites were not rerun.

Fresh replay of all nine patches onto the pinned 0.163.0 base passed in both repositories and produced tree `adae3f6632bcaefe052639a40f0ebe6ee7adab74`. Both changed Rust files match the installed fix’s source byte for byte. Patch application and repository whitespace checks passed.

## Recent sorting

Patch 7 adds Recent grouping (source commit `9c7ad291`). All 81 Agents tests passed with retries disabled, covering timestamp fallback and ties, pins, keyboard navigation, selection after refresh, saved settings, search, archive/delete, hide, and external removals. The Recent rendering snapshot was reviewed; the existing hide-selection snapshot was extended with the new mode’s rows after an initial 80/81 run.

Config schema regeneration, formatting, and whitespace checks passed. Replaying all seven patches produced tree `6a9cf7e67acafd266b3f764409c8cccf9e01996a`; production source matches the implementation commit. The only difference is the launcher fixture argument described above. No fresh CLI package or live installation was made for this change; full workspace tests were not rerun.

## Agents directory fix

The separate patch includes regression coverage for saved local directories and preserved remote behavior. Its original source commit is `5ecb16a2f5c9d34f61aabf4fe75d2cd42c753f8c`, based on Codex 0.162.1. Its Rust tests were not rerun while preparing this repository.
