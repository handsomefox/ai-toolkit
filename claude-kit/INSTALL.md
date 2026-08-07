# Install (Claude Code)

Copy `skills/*` to `~/.claude/skills/`, replacing what is there.

Copy `CLAUDE.md` to `~/.claude/CLAUDE.md`.

Merge `settings.json` into `~/.claude/settings.json`. The file here is the whole
of the current global settings and contains no machine-specific paths, so a
straight copy works on a fresh machine. On an existing one, merge rather than
overwrite: `enabledPlugins` and `permissions` accumulate over time.

Do not add a `hooks` block. The format-on-edit hook this setup used to carry was
removed deliberately — see the README.

Start a new session to confirm the skills load. Only `go-tooling` appears in the
model-facing skill list; the other nine are `disable-model-invocation: true` and
surface as `/` commands.

## Not covered here

Plugin caches under `~/.claude/plugins/`, and session state — `projects/`,
`sessions/`, `history.jsonl`, `file-history/`, `tasks/`, `.credentials.json`.
Those are app-managed. Do not copy them between machines.
