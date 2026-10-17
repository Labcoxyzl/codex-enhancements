# Codex Enhancements

Patches for Codex CLI: a conditional prompt queue and an Agents working-directory fix.

## Conditional queue

Queue prompts or goals, choose when they run, and send them to the current thread or a named new thread.

| Key | Action |
| --- | --- |
| Tab | Queue the composer text, including `/goal` |
| Shift+Left | Open the queue |
| ↑ / ↓ | Select an entry |
| Enter / `e` | Edit its text |
| `t` | Choose when it runs |
| `n` | Choose its destination |
| `a` | Choose prompt, goal, or a supported TUI command |
| Ctrl+↑ / ↓ or Shift+↑ / ↓ | Reorder a pending entry |
| Delete | Cancel an entry |
| Esc | Close the queue |

Triggers include after a response, after the latest response, after the current goal completes, or after an earlier queued goal completes. Destinations include the current thread, a new thread with an optional name, or a thread created by an earlier entry.

For example, while Codex works on a goal, queue “Review the changes,” open the queue, and set its trigger to **After current goal completes**. Choose a named new thread if you want fresh context; include the files or instructions it will need.

Entries dispatch in order. A waiting entry holds later entries, but work in different destination threads can overlap. Tab makes an entry eligible immediately—even when idle—so set conditions before the active response ends. Opening the queue does not pause dispatch. If a completion popup is open, dismiss it before using Tab to queue text.

Each entry can be edited separately. Recovery protects active dispatches from competing servers and holds uncertain deliveries for review instead of automatically resending them.

**Based on Codex 0.163.0.** [Build, install, and rollback](INSTALL.md).

## Agents working-directory fix

Reopening an existing local task from Agents keeps that task’s saved working directory, even if Codex was launched with `--cd` or you used `/cd`. Ordinary resume-picker and remote-environment behavior stays unchanged.

**Separate patch for Codex 0.162.1.** It is not included in the queue series. [Apply the Agents fix](INSTALL.md#agents-working-directory-fix).

[Verification](VERIFICATION.md) · [Apache-2.0 license](LICENSE)
