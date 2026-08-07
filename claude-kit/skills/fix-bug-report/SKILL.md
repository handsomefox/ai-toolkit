---
name: fix-bug-report
description: "Confirm, reproduce, test, fix, and verify a reported defect from the current file, selection, pasted report, or user description. Use explicitly when the user asks to investigate or fix a bug report or invokes /fix-bug-report."
disable-model-invocation: true
allowed-tools: Read, Grep, Glob, Edit, Write, Bash
---

# Fix bug report

Use the current file, selection, pasted text, or user message as the report.

1. Confirm that the described behavior is a bug. State the evidence if it is not reproducible.
2. Add a failing regression test using the closest existing test style. If no suitable layer exists, use the nearest existing suite and say why.
3. Locate and fix the root cause. Keep the fix scoped to the reported defect.
4. Run the regression test and relevant surrounding tests.

If the root cause belongs to a dependency or separately owned repository, identify the owner and prepare a concise handoff: bug summary, reproducer, likely fix area, regression test, and verification. Do not add a downstream workaround unless the user asks.

Report whether the bug was confirmed, the root cause and fix (or owning repository), and verification performed.
