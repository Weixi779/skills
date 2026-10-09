---
name: shape-commits
description: Organize task-related changes into intentional commits using Git or Jujutsu (jj) when the user explicitly asks to commit, split, or describe changes, or invokes `/commit`. Detect the repository workflow, preserve unrelated WIP, and verify the resulting boundaries. Never trigger merely because implementation is complete.
---

# Shape Commits

## Choose the operation

1. Require an explicit request to commit, split, or describe changes. A request to inspect or propose grouping permits analysis only. Never commit automatically after implementation or validation.
2. Read repository instructions and recent subjects. Follow repository-specific message rules; otherwise use Conventional Commits. Do not invoke a repository-specific Git recipe blindly in a jj workspace.
3. Detect the workflow before mutation. Honor the user's explicit tool choice. Check for a jj workspace (including ancestor directories); use `jj root` and read-only inspection when appropriate. A colocated `.git` does not mean Git is the active workflow. If jj is active, use the jj path by default; otherwise use Git. A failed jj command is not permission to fall back to Git mutations.
4. Establish the authorized paths/hunks and current state. “Split this change” refers to the discussed work, not every modification in the workspace. Preserve unrelated WIP, pre-existing staging, local-only documents, and existing conflicts.
5. State the intended grouping briefly and proceed when the boundary is clear. Ask only when an undiscoverable ambiguity affects which content belongs in the operation.

## Shape the groups

Each group should have one purpose that can be explained, validated, and reverted independently. Keep directly supporting tests and documentation with their change. Split larger work when responsibilities warrant it, not to satisfy a fixed size rule.

Capture the baseline before mutation: selected content, remaining changes, file contents/modes, current revision/parent, and relevant public refs. Scale the checks to the task; for mixed jj splits, record the aggregate tree and bookmarks as well. Account for concurrent edits rather than reverting them to make a comparison pass.

## Git workflow

- Inspect `git status`, staged and unstaged diffs, and relevant untracked files before staging.
- Stage explicit paths or hunks only. Do not use `git add .`, `git add -A`, or unverified globs.
- A Git commit consumes the entire index. If unrelated changes are already staged, preserve that selection; do not include it in the commit or unstage it without authorization. Use a verified isolated-index workflow when safe, or ask if the boundary cannot be preserved.
- Review the exact prospective commit diff, then commit without bypassing hooks. Verify its paths/content and the remaining working-tree/index state.
- For “split” in Git, distinguish grouping uncommitted WIP into new commits from rewriting existing commits. Do not reset, amend, or rebase existing history merely because splitting was requested without an identified target and scope.

## Jujutsu workflow

Read [references/jujutsu.md](references/jujutsu.md) before jj mutations. Use explicit filesets or controlled hunk selection, and preserve the remaining WIP in the current change.

**A partial `jj split` already creates the remaining child change and moves `@` there. Stop there after verification. Never append `jj new`, move `@`, or relocate remaining WIP just to obtain an empty working change. An empty follow-up requires a separate explicit user request.**

## Message rules

- Use `<type>[optional scope]: <description>` when Conventional Commits applies.
- Prefer a stable code or domain boundary for `scope`. Follow repository rules when they require an issue key there.
- Add a body when the subject would hide meaningful behavior, motivation, tradeoffs, or several related subchanges.
- Use `Refs: PROJECT-123` as the default Jira-style reference footer when no repository convention overrides it.
- Use issue-closing footers or Jira Smart Commit commands only when explicitly requested or unambiguously required by the repository workflow.
- Use `!` or a `BREAKING CHANGE:` footer for breaking changes.

## Finish at the requested boundary

Verify the selected changes contain only the intended content and the complete workspace content is preserved. Report created hashes/change IDs, subjects, where remaining WIP lives, and validation/hook results. Do not claim tests ran when only content or history checks ran.

Do not push, move branches/bookmarks, modify Git configuration, discard changes, or perform extra history operations without authorization. A requested jj split authorizes that scoped split, not a subsequent `new`, `squash`, rebase, or cleanup. A request to `describe` only authorizes the specified description change.

Do not apply an older memory recipe that contradicts these rules. In particular, “always end with an empty jj working change” is not a requirement.
