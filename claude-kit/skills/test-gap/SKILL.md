---
name: test-gap
description: "Find changed branches that no test meaningfully asserts, ranked by the consequence if they are wrong. Use explicitly when the user asks to review a diff for coverage, identify missing tests before merging, or inspect test gaps in a branch or package. Do not write tests unless asked afterward."
disable-model-invocation: true
argument-hint: [diff, branch, or package]
allowed-tools: Read, Grep, Glob, Bash
---

# Test gap

Scope: staged changes by default, or a named branch or package. Nothing outside
it appears in the output.

## Measure

Run the repo's coverage tooling and read the data. **Never estimate coverage by
reading source.** If coverage tooling is not configured here, say so and stop.

## Cross-reference

Intersect coverage with the diff hunks. A line executed by an unrelated test is
executed, not tested — check that some test asserts on that branch's behavior
rather than merely reaching it. Report those as "executed, unasserted."

## Rank by consequence

Silently wrong results, then data loss, then crashes, then degradation. Error
paths, boundaries, and concurrent access before happy paths.

## Exclude

Generated code, trivial accessors, anything the diff did not touch, and tests
that would assert the implementation matches itself.

## Output

Table: location, uncovered branch, consequence if wrong, nearest existing test,
the assertion that closes it.
