# Install (Codex)

Copy `skills/*` to `~/.agents/skills/`, replacing the toolkit-managed skill
directories already there. Each skill ships a `SKILL.md` and an
`agents/openai.yaml` with its invocation policy.

Copy `AGENTS.md` to `~/.codex/AGENTS.md`.

Copy `rules/default.rules` to `~/.codex/rules/default.rules`. These command
prefixes skip approval prompts and are the Codex counterpart to Claude Code's
`permissions.allow` entries.

Merge `config.toml` into `~/.codex/config.toml`. The kit intentionally excludes
machine-specific credential storage, writable roots, project trust, TUI theme,
and notification setup.

The model may invoke `go-tooling`, `grilling`, `resolving-merge-conflicts`, and
`unslop` automatically. The other ten skills have
`policy.allow_implicit_invocation: false` and remain available through explicit
`$skill-name` invocation.

Formatting, tests, linters, and analyzers run as verification checkpoints after
a coherent edit set.
