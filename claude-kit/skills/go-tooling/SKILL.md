---
name: go-tooling
description: "Go analyzers beyond the standard toolchain for dead code, nil safety, discarded errors, vulnerabilities, struct layout, API compatibility, dependency analysis, and benchmarks. Use when editing, reviewing, or auditing Go files or a repository containing go.mod, especially for go build, go test, go vet, formatting, correctness audits, or performance work."
---

# Go tooling

Beyond `go build`, `go vet`, `go test`, and `gofmt`:

| Purpose | Command |
|---|---|
| Stricter formatting check | `gofumpt -l .` or `gofumpt -d .` |
| Unreachable code | `deadcode ./...` |
| Nil dereference | `nilaway ./...` |
| Discarded errors | `errcheck ./...` |
| Struct padding | `betteralign ./...` |
| Known CVEs | `govulncheck ./...` |
| Syntax modernization | `modernize -fix ./...` |
| Tidy drift | `go mod tidy -diff` |
| API breaks | `apidiff`, `gorelease` |
| Benchmark deltas | `benchstat old.txt new.txt` |
| Escape analysis | `go build -gcflags='-m' ./...` |
| Why a dependency exists | `go mod why <pkg>` |

Run repo-pinned versions with `go tool <name>`, otherwise
`go run <module>@<version>`. Do not `go install` into the environment.

`golangci-lint` subsumes vet, staticcheck, and errcheck where `.golangci.yml`
enables them. Read it before running those separately.

`nilaway` and `betteralign` are absent from most CI configs. Audit-time only.

Do not run `gofumpt -w .` between ordinary edits. Run formatting as part of the
verification checkpoint after a coherent edit set; use a repo-wide write only
for an explicitly requested formatting pass or an approved cleanup.
