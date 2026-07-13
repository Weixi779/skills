---
name: create-pull-request
description: Create and publish a GitHub pull request when the user asks to create, open, raise, or submit a PR. Handle scoped commits, pushing the current branch, and opening a draft PR against the user-specified base branch.
---

# Create Pull Request

1. Read the repository instructions and pull request template.
2. Use the base branch specified by the user. If none was provided, ask instead of guessing.
3. Inspect the working tree and the complete `base...HEAD` diff.
4. Commit only changes belonging to the current task. Follow repository commit rules and preserve unrelated changes.
5. Run validation required by the repository and accurately report anything not run.
6. Check whether the current head branch already has a pull request. Do not create a duplicate.
7. Push the current branch without force unless the user explicitly authorizes rewriting remote history.
8. Generate the title and body from the complete diff, preserving the repository pull request template.
9. Create the pull request as a draft and assign it to `@me`.
10. Verify the resulting URL, base, head, assignee, and draft status.

## Rules

- Default every new pull request to draft. Create a ready pull request only when explicitly requested.
- Do not stage unrelated changes, bypass hooks, add reviewers, or add labels unless explicitly requested.
- Report the commit, pushed branch, pull request URL, and validation results.
