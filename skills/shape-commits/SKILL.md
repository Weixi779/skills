---
name: shape-commits
description: Create one or more intentional Git commits only when the user explicitly asks to commit or invokes `/commit`. Inspect repository rules and task-related changes, form atomic groups, stage safely, follow repository conventions with Conventional Commits as the fallback, and write bodies or footers when useful. Never trigger merely because implementation is complete.
---

# Shape Commits

1. Require an explicit request to commit. Never commit automatically after implementing or validating a change.
2. Read the repository instructions and recent commit subjects. Follow repository-specific commit rules; otherwise use Conventional Commits.
3. Inspect the staged and unstaged state before changing it. Preserve unrelated and pre-existing staged changes unless the user clearly includes them.
4. Group task-related changes into intentional commits. Use judgment rather than fixed line counts or file-type rules. For each group, ask whether it:
   - can be explained independently;
   - can be validated independently;
   - can be reverted independently;
   - serves one purpose.
5. Treat size and complexity as pressure to split. Keep directly supporting tests or small documentation updates with their change when that makes the commit more coherent; use separate `test` or `docs` commits when they have an independent purpose.
6. For each group:
   - stage only its explicit task-related paths or hunks;
   - review the staged scope before committing;
   - write one concise subject for its primary intent;
   - add a body when the subject would hide meaningful behavior, motivation, tradeoffs, or several related subchanges;
   - add footers only when supported by repository or platform conventions;
   - create the commit without bypassing hooks.
7. After all groups are committed, verify the created commits and remaining working-tree state. Report each commit hash and subject, any uncommitted changes, and validation or hook results.

## Message Rules

- Use `<type>[optional scope]: <description>` when Conventional Commits applies.
- Prefer a stable code or domain boundary for `scope`. Follow repository rules when they require an issue key there.
- Use `Refs: PROJECT-123` as the default Jira-style reference footer when no repository convention overrides it.
- Use issue-closing footers or Jira Smart Commit commands only when explicitly requested or unambiguously required by the repository workflow.
- Use `!` or a `BREAKING CHANGE:` footer for breaking changes.

## Safety

- Do not use broad staging commands such as `git add .`, `git add -A`, or unverified globs.
- Do not modify Git configuration, amend commits, skip hooks, rewrite history, or discard changes unless explicitly requested.
- If safe grouping is ambiguous, stop before staging and ask the user.
