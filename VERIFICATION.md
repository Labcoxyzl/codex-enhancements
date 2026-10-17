# Verification

## Repository preparation

Fresh checks passed: queue patch replay, Agents patch application to 0.162.1, the isolated launcher fixture, and local documentation links. The host-specific shell wrapper is excluded.

## Queue

The six patches are based on source commit `c93b64f8` on Codex 0.163.0. The public version uses `--help` in the launcher argument-forwarding fixture; production code is unchanged. Archive replay produced tree `d3e1ce36061c4c94862899fd514f2be50c8c3c94`.

The implementation session recorded:

- 464 focused TUI queue, Tab, and reconnect tests; 70 queue/goal extension tests; 24 app-server integration tests: all passed after correcting three outdated test expectations.
- Launcher install, argument forwarding, fallback, and rollback fixture: passed.
- Isolated TUI checks for independent edits, named destinations, triggers, actions, and both reorder key pairs: passed.
- Cross-server recovery regression: passed.
- Full-duration scenario with two goals and multiple threads: correct routing, no duplicate prompts, both goals complete, queue empty. The monitor initially failed on a model-picker assumption; picker interaction was verified separately.

The final CLI and package built successfully. Earlier packages passed disposable daemon install/restore checks; that check and the long scenario were not repeated after the final focused changes. Full workspace suites and other upstream releases are not covered by these results.

## Agents directory fix

The separate patch includes regression coverage for saved local directories and preserved remote behavior. Its original source commit is `5ecb16a2f5c9d34f61aabf4fe75d2cd42c753f8c`, based on Codex 0.162.1. Its Rust tests were not rerun while preparing this repository.
