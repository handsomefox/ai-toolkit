# AI toolkit

One workflow set, two harnesses. `claude-kit/` and `codex-kit/` hold the same
ten skills plus the global instruction file and configuration each tool reads.
This repo tracks the installed state; edit here, then copy into place.

| | Claude Code | Codex |
| --- | --- | --- |
| Global instructions | `~/.claude/CLAUDE.md` | `~/.codex/AGENTS.md` |
| Skills | `~/.claude/skills/` | `~/.agents/skills/` |
| Configuration | `~/.claude/settings.json` | `~/.codex/config.toml` |
| Command approvals | `permissions.allow` in `settings.json` | `~/.codex/rules/default.rules` |

See each kit's `INSTALL.md` for the copy steps.

## Skills

| Skill | Invocation | Activation |
| --- | --- | --- |
| diff-review | `/diff-review` | Explicit only |
| tech-debt-pass | `/tech-debt-pass` | Explicit only |
| dependency-upgrade | `/dependency-upgrade` | Explicit only |
| test-gap | `/test-gap` | Explicit only |
| commit-split | `/commit-split` | Explicit only |
| go-tooling | `/go-tooling` or matching Go work | Explicit or implicit |
| draft-pr-and-commit | `/draft-pr-and-commit` | Explicit only |
| fix-bug-report | `/fix-bug-report` | Explicit only |
| remove-slop | `/remove-slop` | Explicit only |
| caveman | `/caveman` | Explicit only |

Codex invokes skills as `$name` rather than `/name`.

`go-tooling` is the only skill the model may load on its own, so Go work picks
up the analyzer table without being asked. The other nine stay explicit — they
are workflows you choose, not background knowledge.

Codex disables five skills through `[[skills.config]]` in `config.toml`:
caveman, dependency-upgrade, remove-slop, tech-debt-pass, test-gap. Claude Code
has no per-skill toggle, so all ten are live there; delete the directory to
match.

## Shared decisions

**No format-on-edit hook.** A post-edit formatter rewrites the file underneath
the model, which forces a re-read and costs a tool call on every single edit.
Formatting is checkpoint work instead: run it once after a coherent edit set.
Both instruction files say so, and `go-tooling` prefers `gofumpt -l .` over
`gofumpt -w .` for the same reason.

**Memory off.** `autoMemoryEnabled = false` on Claude, `generate_memories` and
`use_memories` false on Codex. Accumulated memories are billed as context on
every turn and drift out of date faster than they earn their keep.

**Compaction below the long-context tier.** Codex compacts at 272000 tokens,
scoped to the total including any carried prefix, to stay under GPT-5.6's
long-context pricing step.

## Divergence from the two harnesses

The instruction files differ only where the tooling does. Codex gets `rg`,
`rg --files`, and `apply_patch`; Claude Code gets Grep, Glob, and Edit/Write,
because its built-ins return structured results and do not consume permission
prompts. Everything downstream of that — `ast-grep`, LSP for symbol queries,
`jq`/`yq`, `fd` for file metadata, non-interactive commands, `tldr` — is
identical.

Claude Code skills carry frontmatter Codex expresses in `agents/openai.yaml`:
`disable-model-invocation` maps to `policy.allow_implicit_invocation`.
`allowed-tools` and `argument-hint` have no Codex counterpart; on Claude they
enforce what the skill prose only asserts, such as `diff-review` and
`draft-pr-and-commit` being unable to edit files.
