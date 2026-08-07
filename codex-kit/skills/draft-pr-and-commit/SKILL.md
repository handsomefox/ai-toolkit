---
name: draft-pr-and-commit
description: "Draft a PR title, PR description, and commit message from the current branch diff and ticket context. Use explicitly when the user asks to prepare PR copy, a commit message, release notes for a change, or invokes $draft-pr-and-commit. Draft only; never commit or open a PR."
---

# Draft PR and commit

Read the branch diff and ticket context from the branch name, commits, or user input. Output three labelled, copy-ready blocks: PR title, PR description, and commit message.

## Style

- Commit message: Conventional Commits; lowercase imperative subject; no ticket prefix. Example: `fix(parser): reject malformed input`.
- PR title: If a ticket such as `PROJ-123` is known, use `PROJ-123: Sentence-case summary`; otherwise use a concise sentence-case summary without a ticket prefix.
- PR description: Start with `Ticket: <link>` or `Fixes <link>` when a ticket exists, then explain the change plainly and technically. Use inline code for symbols, environment variables, and commands.

Scale the description to the change:

- Trivial: ticket line only.
- Small: one or two paragraphs and, when relevant, `## Testing`.
- Larger: summary followed by a numbered `Other changes:` list. Use `→` for renames.

Add `## Background`, `### Breaking changes`, `### Behaviour / compatibility`, `## Demos`, or image details only when they help the reader.

## Examples

Commit: `fix(catalog): display parsed values correctly`

PR title: `PROJ-123: Reject conflicting availability updates`

Small PR description:

```markdown
Ticket: <https://tracker.example.com/browse/PROJ-123>

Reject update requests that set `is_enabled` to a value that conflicts with `enabled_versions`.

`enabled_versions` remains the source of truth, preventing an inconsistent value from being accepted and later autocorrected by validation.

## Testing

Added JSON and Go test cases and ran `make test`.
```

## Rules

- Never invent ticket IDs, links, test results, or release/version prefixes.
- Use no emojis, tables, or nested Markdown. Use plain paragraphs plus `-` or numbered lists.
- Preserve project-specific terminology and conventions found in the diff, ticket, and repository guidance.
