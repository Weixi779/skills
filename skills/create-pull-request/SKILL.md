---
name: create-pull-request
description: Create and publish a GitHub pull request when the user asks to create, open, raise, or submit a PR. Prevent duplicates, inspect the complete change scope in bounded batches, commit only task-related work, push safely, and open a draft PR against the user-specified or unambiguous repository base branch.
---

# Create Pull Request

1. Read the repository instructions and applicable pull request template.
2. Resolve the base branch from the user's request or an unambiguous repository instruction; otherwise ask. Verify that `origin/<base>` exists and is current.
3. Identify the current head branch. Stop for detached HEAD or when head equals base.
4. Check for an existing open pull request from the current head. Stop instead of creating a duplicate.
5. Inspect the working tree, commit subjects, changed paths, and diff statistics against `origin/<base>...HEAD`.
6. Account for every changed path. Inspect textual diffs in bounded batches; use statistics for generated files, binaries, and assets unless their contents matter to the task.
7. If task-related changes are uncommitted, stage only their explicit paths or hunks, review the staged diff, commit according to repository rules, and repeat the change summary. Preserve unrelated changes.
8. Run repository-required validation and accurately report anything not run.
9. Generate the title and body from the reviewed change set while preserving the applicable pull request template.
10. Push the current branch without force, create a draft pull request assigned to `@me`, and verify its URL, base, head, assignee, and draft status.

## Rules

- Default every new pull request to draft. Create a ready pull request only when explicitly requested.
- Do not stage unrelated changes, bypass hooks, add reviewers, or add labels unless explicitly requested.
- Report the commit, pushed branch, pull request URL, and validation results.
