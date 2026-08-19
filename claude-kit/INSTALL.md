# Install (Claude Code)

Copy `skills/*` to `~/.claude/skills/`, replacing the toolkit-managed skill
directories already there.

Copy `CLAUDE.md` to `~/.claude/CLAUDE.md`.

Merge `settings.json` into `~/.claude/settings.json`. The file here contains the
stable global settings and no machine-specific paths. On an existing machine,
merge rather than overwrite because plugin and permission settings can diverge.

Do not add a format-on-edit hook. Formatting is a verification checkpoint after
a coherent edit set.

The model may invoke `go-tooling`, `grilling`, `resolving-merge-conflicts`, and
`unslop` automatically. The other ten skills set
`disable-model-invocation: true` and remain available as explicit `/` commands.

Claude Code watches existing skill directories for changes. Start a new session
only when creating a top-level skills directory that was absent when the session
started.

## Not covered here

Plugin caches under `~/.claude/plugins/` and session state such as `projects/`,
`sessions/`, `history.jsonl`, `file-history/`, `tasks/`, and credentials are
app-managed. Do not copy them between machines.
