## Tools

Use `rg` for text search and `rg --files` for file discovery. Use `apply_patch`
for deliberate file edits. These are fast, respect ignore files, and keep edits
reviewable.

Reach for specialized tools where text search cannot do the job:

- **Structural search**: `ast-grep --lang <language> -p '<pattern>'` matches
  code shape rather than text. Use it for queries `rg` cannot express — every
  call to a function with a particular argument shape, every error return that
  discards the error, every struct literal missing a field. `-l` lists matching
  files only.
- **Symbol queries** — definition, references, implementations, call
  hierarchy — go through the LSP tools, not `ast-grep` and not `rg`. LSP
  resolves through aliases, embedding, and re-exports; text and AST search do
  not.
- **JSON**: `jq`. **YAML, XML, TOML**: `yq`.
- `fd` when you need file metadata `rg --files` does not expose: modification
  time, size, type, permissions.

Every command must be non-interactive and must terminate. Add `--filter`,
`head`, `-n`, `--json`, or `--no-pager` as the tool requires; `git` needs
`--no-pager` for anything that would page. A command that waits on input hangs
the session with no way to recover except interrupting the turn.

For an unfamiliar tool: `tldr <tool>`, or `<tool> --help` if tldr has no page.

## Tooling

Treat formatting, tests, linters, and analyzers as checkpoint work. After a
coherent set of edits, run the appropriate formatter and the full relevant test
and lint suites once. Use narrow checks only to diagnose a specific failure;
do not run broad verification between edits.

Before committing, a repo-wide format pass is fine when it covers additional
languages or project-wide formatting requirements.
