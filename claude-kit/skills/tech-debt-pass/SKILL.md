---
name: tech-debt-pass
description: "Audit a bounded scope for correctness risk, complexity, dead code, inconsistency, and measured performance problems, then report before changing anything. Use when the user asks to simplify, refactor, clean up, or speed up a specific package, directory, or diff. Do not use for feature work, bug fixes, or whole-repository sweeps."
disable-model-invocation: true
argument-hint: [package or directory path]
allowed-tools: Read, Grep, Glob, Edit, Bash
---

# Tech debt pass

## Scope

If no path was given, ask. Do not pick.

Ceiling: one package, ~15 source files, or one diff. Larger requests get a
proposed split, not a best effort.

## Baseline

Use the tooling the repo already pins — CI config, Makefile, lint config,
manifest scripts. Do not install anything globally without asking.

Run build, type check, lint, and tests, including a race or concurrency detector
when the repository and language support one. **Red baseline means stop.**

## Analyzers CI does not run

The value is in what a model reading source cannot see. Beyond the repo's lint:

| Concern | Go | Rust | TypeScript | Python |
|---|---|---|---|---|
| Dead code | `deadcode` | `cargo-udeps` | `knip` | `vulture` |
| Nil safety | `nilaway` | — | `strictNullChecks` | `mypy --strict` |
| Discarded errors | `errcheck` | `#[must_use]` | `no-floating-promises` | — |
| CVEs | `govulncheck` | `cargo audit` | `osv-scanner` | `pip-audit` |
| Layout | `betteralign` | `cargo bloat` | — | — |
| Cycles | `go mod graph` | `cargo tree -d` | `madge -c` | `pydeps` |

Other languages: find the equivalent. `semgrep` covers most for pattern rules.

Report which you ran and which are unavailable here.

## Audit, no edits

Per finding: `file:line`, the problem, cost (S/M/L), risk (low/med/high).

1. Discarded errors, unchecked casts, leaked goroutines/threads/handles,
   cancellation not propagated, locks held across I/O, unsynchronized state
2. Duplicated logic — list every site; two blocks differing by one condition
   are not duplication
3. One problem solved two ways in one module — name the canonical one
4. Unreachable branches, unused exports, commented-out code
5. Abstractions defined at the producer not the consumer; interfaces wider
   than any caller uses
6. Performance — evidence only

Prioritize by churn against complexity:
`git log --format=%H --since=1.year -- <path>` per file. Complexity nobody
touches is cheaper to leave than complexity edited weekly.

State which findings you recommend **not** acting on.

Never write "consider extracting X." Name the change or drop the finding.

## Performance

No claim without a benchmark, profile, or complexity argument tied to a real
input size. Report repeated-run before/after, not one measurement. If no
benchmark covers the code, say so and offer to write one. "Avoids an
allocation" is not a result.

## Apply approved findings

- One category per edit. Never mix a rename with a restructure.
- Re-run the baseline after each; show before/after.
- **A test that must change to pass is a behavior change. Stop and explain.**
- No new dependencies. No abstraction under three call sites.
- Do not touch exported identifiers unless asked — external callers are
  invisible from here.
- Preserve error strings and log messages unless the finding was about them.
- Mechanical auto-fixes land as their own edit, never interleaved.

## Output

What changed, and what was left alone with the reason.
