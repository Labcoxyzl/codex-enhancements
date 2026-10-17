# Installation

## Conditional queue

Tested on Linux/WSL with Codex `rust-v0.163.0` (`83086188ae0614bd882818ff1aa23f3194962012`). Other releases require a rebase and fresh testing.

### Apply and build

Start with Git, Python 3, `just`, and the Rust/build prerequisites listed in the pinned Codex checkout’s `docs/install.md`. Clone this repository and Codex into sibling directories:

```bash
git clone https://github.com/Labcoxyzl/codex-enhancements.git
git clone --branch rust-v0.163.0 https://github.com/openai/codex.git codex-queue
cd codex-queue
git switch -c conditional-queue
git am ../codex-enhancements/patches/conditional-queue/*.patch
just assemble-codex-package --cargo-profile release --package-dir "$HOME/.local/share/codex-enhancements/0.163.0"
```

The package builder includes the CLI and required helpers. Keep the complete package directory; copying only the CLI executable is insufficient.

### Activate

Finish active turns and queued work first: updating the daemon restarts it. The CLI and daemon must both use the patched build.

From the patched Codex checkout:

```bash
"$HOME/.local/share/codex-enhancements/0.163.0/bin/codex" app-server daemon update --from-cli --yes
scripts/local-codex-launcher.sh install "$HOME/.local/share/codex-enhancements/0.163.0/bin/codex"
scripts/local-codex-launcher.sh status
```

The launcher expects `~/.local/bin/codex` to already be a symlink to your official Codex executable, with that directory on `PATH`. Check this before activation. It refuses to overwrite a regular file and retains the original target for rollback. If your installation uses a different layout, adapt it before running these commands.

Relaunch clients after activation. Existing shell aliases or functions can still override `codex`; check `type -a codex` and remove any unwanted argument-injecting wrapper yourself. This repository does not install one.

### Roll back

At another quiet boundary, from the same checkout:

```bash
scripts/local-codex-launcher.sh rollback-all
```

This restores the official daemon, then the original CLI link. Keep the original official package and `~/.local/state/codex-queue/launcher-targets` until rollback completes.

## Agents working-directory fix

Use a separate checkout at `rust-v0.162.1`; do not stack this older patch onto the queue checkout without porting and testing it.

```bash
git clone --branch rust-v0.162.1 https://github.com/openai/codex.git codex-agents
cd codex-agents
git switch -c agents-saved-directory
git am ../codex-enhancements/patches/agents-cwd/*.patch
```

Build and run using that checkout’s `docs/install.md`. The patch changes Agents task reopening; no new command or setting is needed.
