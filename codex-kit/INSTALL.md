# Install (Codex)

Copy `skills/*` to `~/.agents/skills/`, replacing what is there. Each skill
ships a `SKILL.md` and an `agents/openai.yaml` carrying its invocation policy.

Copy `AGENTS.md` to `~/.codex/AGENTS.md`.

Copy `rules/default.rules` to `~/.codex/rules/default.rules`. These are the
command prefixes that skip an approval prompt — the Codex counterpart to
`permissions.allow` in the Claude kit. They are not part of the original kit.

Merge `config.toml` into `~/.codex/config.toml`, and expand the `~` in the
`[[skills.config]]` paths — Codex wants them absolute.

Do not copy app-managed configuration from another machine, including desktop
preferences, MCP paths, marketplace sources, project trust, connector IDs, and
workspace-specific writable paths. The `config.toml` here already excludes them:
credential store, `writable_roots`, `[projects.*]`, `[tui]`, and notification
setup all stay local.

Start a new Codex task to confirm the five enabled skills are available.
Formatting, tests, linters, and analyzers run as verification checkpoints after
a coherent edit set.
