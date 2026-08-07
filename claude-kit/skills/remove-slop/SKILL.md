---
name: remove-slop
description: "Remove unnecessary AI-style code from the current branch while preserving behavior and local conventions. Use explicitly when the user asks to clean up AI-generated code, reduce boilerplate, remove overengineering, or invokes /remove-slop."
disable-model-invocation: true
allowed-tools: Read, Grep, Glob, Edit, Bash
---

# Remove slop

Compare the current branch with its default branch. Remove only branch-introduced excess that conflicts with surrounding code:

- Comments that restate code or do not match local comment style.
- Defensive checks, assertions, or error wrapping unsupported by the surrounding trusted/internal code path.
- Helpers, interfaces, wrappers, or configuration created for one call site.
- Verbose names that break local naming conventions.
- Unnecessary imports, initialization, or dead scaffolding.

Do not change behavior, remove intentional validation, rename public API symbols, or perform unrelated refactors. When intent is unclear, leave it intact.

Run the relevant focused verification after edits. Finish with a 1–3 sentence summary of what was removed and the verification run.
