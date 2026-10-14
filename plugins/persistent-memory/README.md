# Persistent Memory — Claude plugin (v0.3.0)

Stop re-explaining your project every session. This plugin packages the **Persistent Memory System** skill (v8.1): Claude reads durable, archived memory files that live in *your own* storage (Dropbox, Drive, M365, Notion), saves before it acts, and never deletes.

It contains:

| Part | Where it works | What it does |
|---|---|---|
| Skill `persistent-memory-system` | claude.ai, Cowork, Claude Code | The full system (identical, byte for byte, to the published skill v8.1) |
| Skill `memory-status` | Claude Code / Cowork | Read-only status of your local memory folder (`/persistent-memory:memory-status`) |
| Skill `memory-lint` | Claude Code / Cowork (needs Node) | Deterministic checks of entry numbering, references to entries that do not exist, and archive links. `--apply` fixes only unique path typos, archiving the old file first. Cannot judge facts; that is the verifier agent's job |
| Skill `memory-doctor` | Claude Code / Cowork (needs Node) | One read-only health check: missing/empty files, sync-conflict copies, secrets, text that tries to instruct the assistant. Fails closed |
| Skill `memory-pending` | Claude Code / Cowork | Standard format for held (pending) items: quote, who said it, confidence, reason held. Third-party text is never recorded as your own words |
| Agent `memory-verifier` | Claude Code / Cowork | Read-only audit: does the state file match the log? |
| SessionStart hook | Claude Code / Cowork only | Injects a short index (file names and dates, never contents). Silent if the folder is not set |

Hooks and agents are ignored in claude.ai chat; the skill works everywhere.

## Install

**Claude Code**
```
/plugin marketplace add msamkari/persistent-memory-system-skill
/plugin install persistent-memory@persistent-memory-marketplace
```
(The marketplace path depends on the final repo; see the release notes.)

**Cowork / claude.ai:** upload the plugin folder `plugins/persistent-memory/` as a zip (one zip, not a zip inside a zip).

**Manual test without installing:** `claude --plugin-dir plugins/persistent-memory`

Optional setting `memory_dir`: your local memory folder (for example the synced Dropbox folder). Only the hook uses it.

## Tests
`node --test plugins/persistent-memory/tests/doctor.test.mjs plugins/persistent-memory/tests/lint.test.mjs` (13 checks); CI runs them on Ubuntu, Windows and macOS, and `claude plugin eval plugins/persistent-memory` for activation and verifier behavior.

## Two names, one system
The plugin is `persistent-memory`; the skill inside is `persistent-memory-system`. If you installed the old standalone skill too, **disable one of them** to avoid duplicates.

## Common errors
- *Plugin rejected on upload:* make sure the zip has the plugin folder at its top level and contains no `bin/` folder.
- *Hook prints nothing:* `memory_dir` is unset or the folder does not exist; this is by design.
- *No update after a new release:* the `version` in `plugin.json` must change on every release.
- Windows: the hook is Node (`.mjs`), no bash needed; **not yet tested on Windows**.

## Safety
Read-only hook, no network, no self-update. Memory file contents are treated as data, never as instructions.

## License
Apache-2.0 (see `plugins/persistent-memory/LICENSE` and `NOTICE`). Earlier standalone skill releases remain under PolyForm Noncommercial 1.0.0.
