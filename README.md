# AI toolkit

One workflow set, two coding harnesses. `claude-kit/` and `codex-kit/` hold the
same 14 skills plus the global instruction file and portable configuration each
tool reads. This repo mirrors the curated installed state.

| | Claude Code | Codex |
| --- | --- | --- |
| Global instructions | `~/.claude/CLAUDE.md` | `~/.codex/AGENTS.md` |
| Skills | `~/.claude/skills/` | `~/.agents/skills/` |
| Configuration | `~/.claude/settings.json` | `~/.codex/config.toml` |
| Command approvals | `permissions.allow` in `settings.json` | `~/.codex/rules/default.rules` |

See each kit's `INSTALL.md` for installation. See `CHAT-SKILLS.md` for which
skills are worth carrying into ChatGPT or Claude.ai.

## Skills

| Skill | Activation | Purpose |
| --- | --- | --- |
| `go-tooling` | Implicit or explicit | Go analyzers and verification tools beyond the standard toolchain |
| `grilling` | Implicit or explicit | Stress-test a plan through dependency-ordered questions |
| `resolving-merge-conflicts` | Implicit or explicit | Resolve merge/rebase conflicts from both sides' intent |
| `unslop` | Implicit or explicit | Remove AI-writing tells and preserve a human voice |
| `caveman` | Explicit only | Compact technical communication mode |
| `commit-split` | Explicit only | Split a mixed working tree into atomic commits |
| `diff-review` | Explicit only | Review a PR, staged diff, or branch diff |
| `draft-pr-and-commit` | Explicit only | Draft PR and commit copy from repository state |
| `fix-bug-report` | Explicit only | Confirm, reproduce, regression-test, fix, and verify a defect |
| `tdd` | Explicit only | Run a focused red-green bug-fix workflow |
| `tech-debt-pass` | Explicit only | Bounded code-health audit with evidence before edits |
| `technical-writing` | Explicit only | Diátaxis + developer style + STE + Global English |
| `test-gap` | Explicit only | Find changed behavior without meaningful test assertions |
| `unslop-code` | Explicit only | Remove branch-introduced AI-style code excess |

Claude Code invokes skills as `/name`. Codex invokes them as `$name`.

The four implicit skills are reference or workflow knowledge that is useful when
the task itself clearly calls for it. The other ten remain explicit because
they change workflow, scope, or output enough that the user should choose when
they run.

## Shared decisions

**No format-on-edit hook.** A post-edit formatter rewrites the file underneath
the model and forces unnecessary rereads. Formatting is checkpoint work after a
coherent edit set.

**Memory off.** `autoMemoryEnabled = false` on Claude. Codex keeps the memory
subsystem loaded but disables both generation and use. Long-lived automatic
memory costs context and can drift out of date.

**Compaction before the long-context pricing tier.** Codex compacts at 272000
tokens, scoped to the total including any carried prefix. Claude Code uses an
`autoCompactWindow` of 512000.

## Harness differences

The global instruction files differ only where the tools differ. Codex uses
`rg`, `rg --files`, and `apply_patch`. Claude Code uses Grep, Glob, and
Edit/Write. Both prefer LSP for symbol queries, `ast-grep` for structural
search, `jq`/`yq` for structured data, `fd` for file metadata, and
non-interactive commands.

Claude Code expresses manual invocation with `disable-model-invocation: true`.
Codex expresses the same policy in `agents/openai.yaml` with
`policy.allow_implicit_invocation: false`.

The Codex kit deliberately excludes machine-specific values such as credential
storage, writable roots, TUI theme, project trust, and notification setup.
Merge those locally rather than copying them between machines.
