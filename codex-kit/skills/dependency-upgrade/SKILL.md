---
name: dependency-upgrade
description: "Upgrade one dependency across a version range by reading all applicable changes, inventorying affected call sites, applying the migration, and verifying it. Use explicitly when the user asks to bump a library, resolve a dependency advisory, or migrate a major version. Do not use for adding a new dependency or routine lockfile refreshes."
---

# Dependency upgrade

One dependency per run. If the resolver drags others along, list them and stop.

## 1. Read every intermediate version

Release notes for the whole range, not just the target. No changelog: diff the
tags. **If you cannot find breaking-change information and the major moved, say
so.** Do not proceed as though it is safe.

Output: breaking changes, each marked applicable or not.

## 2. Inventory call sites

Every symbol in the applicable set. LSP find-references where available. Report
counts per symbol before editing.

## 3. Baseline

Build, test, vulnerability scan at the current version. Red means stop.

## 4. Apply

- Never silence a compile error with a cast, `any`, or a deleted assertion. A
  call that will not migrate is a stop condition, not a workaround.
- Nothing unrelated to this upgrade.
- No other pinned versions move unless the resolver forces it; if forced, list
  what moved and why.

## 5. Verify

Re-run step 3 with the race detector. For an advisory, acceptance is the
scanner going quiet — not a green build.

## Output

Version delta, applicable breaking changes, call sites touched per symbol,
scanner before/after, anything unmigrated.
