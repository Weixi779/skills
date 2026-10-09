---
name: maintain-pull-request
description: Inspect and maintain an existing GitHub pull request from Git or Jujutsu (jj) when the user asks to review feedback, fix checks, rebase or update the branch, reply to or resolve comments, or update PR metadata. Preserve unrelated WIP and publish only the identified PR head.
---

# Maintain Pull Request

## Establish scope and identity

1. Read repository instructions and identify the exact PR, base/head repositories and refs, and current remote head SHA. Attach the PR to the chat when that capability is available.
2. Honor explicit tool choice; otherwise detect an active jj workspace before selecting commands. Detached Git HEAD is normal in colocated jj and is not proof that the PR branch is missing. Resolve the PR's actual local branch or bookmark/revision; never assume `@`, `@-`, or Git HEAD is its head.
3. Inspect the complete PR diff, description, commits, checks, and thread-aware review state. Judge comments against the actual code flow and requirements; reviewer suggestions are not automatically requirements.
4. Respect authorization:
   - Inspection stays read-only.
   - Fix requests permit local modification and validation; publication requires the user's request.
   - Commit, push, reply, and resolve only when requested. A rebase request alone does not authorize publishing rewritten history.
   - Treat response/resolution steps below as conditional on that authorization, not automatic follow-ups to reading a PR.

## Change and verify locally

5. Snapshot relevant WIP content, index state, current/parent revisions, and refs before history changes. Preserve unrelated work and existing conflicts. If the current checkout is not suitable, use an isolated Git worktree or jj workspace following repository instructions. Never initialize a new jj repository inside an existing Git worktree merely to obtain isolation.
6. Make the scoped fix and run relevant validation. When committing/splitting is authorized, use `shape-commits` when available, or preserve the same invariants: explicit paths/hunks, review the resulting diff, preserve unrelated staged content/WIP, and never append `jj new` after a partial split.
7. Rebase only an identified revision set onto an identified base:
   - Git: use the appropriate Git workflow in a suitable checkout, preserve WIP, and do not include other local branches implicitly.
   - jj: inspect the descendant graph before choosing rebase options. `-s`, `-b`, and `-r` have different scopes; even unselected descendants may be rewritten to preserve graph relationships. Never run a default `jj rebase` or assume selecting a source isolates unrelated descendant WIP. A second workspace in the same jj repository shares history and does not prevent those rewrites. If the operation would affect unrelated changes, construct an isolated copy/checkout of the PR history or establish that boundary with the user first.
   - Check installed command help for version-dependent syntax. Verify the resulting graph, conflict state, selected changes, and remaining WIP; do not resolve unrelated conflicts or move unrelated bookmarks as cleanup.
8. Review the complete merge-base-to-publication-tip diff after changes. Use the explicit PR tip rather than blindly `base...HEAD`. Confirm no unrelated ancestors or WIP would be published. Preserve the PR's Draft/Ready state unless the user requests otherwise.

## Publish and respond when authorized

9. Recheck the remote head against the previously reviewed SHA before publishing. If it advanced, inspect and reconcile within scope; do not refresh a lease and overwrite newly discovered work blindly.
   - Git: push only the named PR branch. For an authorized history rewrite, use `--force-with-lease=refs/heads/<head>:<reviewed-remote-sha>`; never unconditional force.
   - jj: point only the intended PR bookmark at the verified revision. Preview with `jj git push --remote <remote> --bookmark 'exact:<head>' --dry-run`, then push only that bookmark. jj checks remote state against its fetched state; do not copy Git force flags into jj, bypass safeguards, or push all/tracked bookmarks. Verify the installed syntax first.
10. Verify the remote/PR head equals the intended commit and the remaining local WIP and unrelated refs are preserved. Do not create an empty working change or move `@` merely because publication finished.
11. When replies/resolution were requested, explain each handled thread's concrete outcome. Resolve only after the fix is pushed or a no-change conclusion is explained. Reply at PR level for feedback without a resolvable thread. If later requirements invalidate an earlier reply/fix, explain the correction in that thread. Use structured arguments or a body file for multiline messages.
12. Report actual local changes, commits, pushes, replies, resolutions, checks, and remaining issues separately. Do not imply that local fixes have been published.
