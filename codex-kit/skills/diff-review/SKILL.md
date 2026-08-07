---
name: diff-review
description: "Review a pull request, staged changes, or a branch diff and report actionable findings by severity. Use explicitly when the user asks to review a PR, review staged changes, or check a diff before pushing. Review only; do not fix findings unless asked separately."
---

# Diff review

Review only. **Do not edit files**, even for something trivial.

## Scope

| Argument | Source |
|---|---|
| `staged` (default) | `git diff --staged` |
| `pr` | `gh pr diff` plus `gh pr view --json title,body,baseRefName` |
| `<base-ref>` | `git diff <base>...HEAD` — three dots, from the merge base |

Skip generated files, vendored code, and lockfiles. Name them as skipped rather
than reviewing them.

## Read past the diff

A diff shows what changed, not what breaks. For every changed signature,
exported symbol, error value, or struct field, find its callers with LSP
references and check them. Breakage lives in files the diff does not contain.

For a PR, read the description and the linked ticket. Report anything claimed
but not implemented, and anything implemented but out of scope.

## Findings

Every finding names the input or state that triggers it. If you cannot name
one, it goes under Questions, not Findings.

Report nothing the repo's formatter or linter already catches — check
`.golangci.yml`, `.eslintrc`, `ruff.toml`, or the CI config first.

Severity:

| | |
|---|---|
| Critical | Data loss, corruption, credential exposure, injection |
| High | Silently wrong results, or a crash on reachable input |
| Medium | Wrong under uncommon conditions, unhandled error, resource leak, race |
| Low | Maintainability cost you can state concretely |

Sort by severity. Within a severity, sort by file.

## Output

```
| Severity | Location | Finding | Proposed fix |
```

Location is `file:line`. Proposed fix is the change, not a direction to explore.

Then:

- **Questions** — things that look wrong but depend on context you lack
- **Not reviewed** — skipped paths and anything you could not resolve

If there are no findings, say so in one line. Do not pad the table.
