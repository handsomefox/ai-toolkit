---
name: commit-split
description: "Turn a mixed working tree into an ordered sequence of atomic commits, each independently buildable and revertable. Use explicitly when the user asks to split unrelated changes, prepare commits before a PR, or clean up unpushed working-tree changes. Do not use to rewrite existing commits."
---

# Commit split

Read `git diff`, `git diff --staged`, and untracked files. Work from the whole
tree.

## Group and order

By intent, not by file — one commit, one reason to change. A file may
contribute hunks to several. Formatting and generated churn gets its own commit,
ordered first.

**Each commit must build and pass tests standing alone, in sequence.** If two
groups cannot separate without breaking that, merge them and name the
dependency that forced it.

## Propose, then stop

Per commit: imperative subject under 50 chars, body explaining why, exact files
or hunks. Wait for approval.

## Execute

Stage by path. If hunk-level staging is needed and cannot run
non-interactively, output the `git add -p` sequence for the user rather than
approximating it.

- Never amend, rebase, or reorder existing commits.
- Never `git checkout` or `git restore` over uncommitted work.
- End by verifying the tree is clean and the final state matches the original
  diff exactly.
