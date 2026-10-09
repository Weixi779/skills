---
name: create-pull-request
description: Create and publish a GitHub pull request from a Git or Jujutsu (jj) repository when the user asks to create, open, raise, or submit a PR. Prevent duplicates, inspect the complete selected change scope, preserve unrelated WIP, push only the intended branch or bookmark, and default to a draft PR.
---

# Create Pull Request

## Resolve the repository and target

1. Read repository instructions and the applicable PR template. Honor explicit tool choice; otherwise detect an active jj workspace before choosing a Git workflow. A colocated `.git` alone does not imply Git. A failed jj command is not permission to mutate with Git instead.
2. Resolve the destination repository, remote, and base branch from the request or unambiguous repository context. Verify the fetched base is current. Do not assume `origin` is the destination in a fork workflow.
3. Resolve a distinct publication head:
   - **Git:** identify the intended branch and its exact tip. Detached HEAD requires resolving or creating an authorized publication branch, not guessing which commit to publish.
   - **jj:** identify the intended revision and publication bookmark. Detached Git HEAD is normal; neither Git HEAD, `@`, nor `@-` alone establishes PR scope. Inspect their ancestry and contents. Ask only if the publication target remains ambiguous.
   - Follow the user's exact branch/bookmark name, then repository naming rules. Create a scoped publication branch/bookmark when needed for the requested PR. Do not repurpose an existing unrelated name or use the base branch as head.
4. Check for an existing open PR using the head repository/owner and branch name. Do not create a duplicate; report the existing PR and maintain it only within the user's requested scope.

## Prepare the complete PR scope

5. Inspect the working state, commit subjects, changed paths, and complete base-to-publication-tip diff. In Git use the resolved base/head, not blindly `HEAD`. In jj inspect the selected ancestry and merge-base-to-tip content; use explicit revisions for colocated Git reads. Review all commits the head would publish, not merely the newest change. Unrelated ancestors are part of PR scope too.
6. Account for every changed path. Inspect textual diffs in bounded batches; use statistics for generated files, binaries, and assets unless their contents matter to the task.
7. If requested content still lives in mixed WIP, isolate only authorized paths/hunks. Use `shape-commits` when installed; otherwise follow the same boundary directly:
   - Git: inspect the index, preserve unrelated staging, stage/review only selected content, and commit without bypassing hooks.
   - jj: use an explicit fileset or controlled hunk split and review the resulting revision. Leave remaining WIP in `@`; never append `jj new` or require an empty workspace. Do not use `git commit`, reset, or checkout to finalize jj changes.
   - Re-resolve the publication tip after any split and recheck the entire PR diff.
8. Run repository-required validation. Write the title/body from the reviewed change set and preserve the PR template. Accurately report anything not run.

## Publish only the selected head

9. Record the current remote head before changing it. A new PR request does not authorize overwriting divergent published history.
   - Git: push only the resolved head branch to the selected remote without force.
   - jj: create/set only the intended bookmark at the reviewed revision within this PR's scope. Verify installed command syntax and use a scoped dry run, then push the exact bookmark, for example `jj git push --remote <remote> --bookmark 'exact:<name>' --dry-run`, followed by the same command without `--dry-run`. Inspect the proposal for unexpected updates/deletions or a non-fast-forward replacement; do not authorize those implicitly.
   - Never use a bare/broad jj push, `--all`, `--tracked`, or bypass empty-description/private-commit safeguards to make an incorrect target pushable. A bookmark excludes descendants but includes all its ancestors; confirm no unrelated WIP is included.
10. Create a draft PR assigned to `@me`, explicitly passing the base and head instead of relying on CLI branch inference. Use structured arguments or a body file for multiline text. Verify its URL, repository, base, head SHA, assignee, and draft status. Attach the created PR to the chat when that capability is available.
11. Verify remaining WIP content is preserved and only intended refs moved. Report the published commit, branch/bookmark, PR URL, and validation results. Pushing does not require moving `@` or creating another working change.

## Rules

- Create a ready PR only when explicitly requested.
- Do not include unrelated changes, bypass hooks, add reviewers/labels, or change repository configuration without authorization.
- If the remote moves concurrently or push is rejected, inspect the new state. Do not silently force, broaden the push, or rebase unrelated work.
