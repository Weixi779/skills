# Jujutsu change boundaries

## Inspect before mutation

Use `jj status`, `jj log`, and `jj diff` to establish the current change, parents, scope, and conflicts. Initial inspection can use `--ignore-working-copy` to avoid snapshot writes, but it may omit unsnapshotted filesystem changes; obtain a fresh snapshot before splitting. Record its commit ID, aggregate tree, selected content, remaining WIP, bookmarks, and public Git refs. Do not compare historical timestamps as evidence that content changed.

For colocated Git, inspect Git HEAD and index too. jj normally exports its working change's parent as Git HEAD; its current WIP is still stored as a Git commit object. Git object existence alone does not imply a branch points to it or that it was published.

## Partial split of the current mixed WIP

Use explicit filesets when whole files belong to the task:

```sh
jj split -m 'fix(scope): describe the selected change' path/to/selected-file
```

Use controlled interactive hunk selection when a file also contains unrelated work. Inspect the selected diff before accepting it. Verify installed `jj split --help` before using version-dependent options, especially for non-current revisions or insertion positions.

For the default sequential partial split:

```text
before: base -> @ (selected work + unrelated WIP)
after:  base -> selected change -> @ (remaining WIP)
```

Verify:

- `jj diff -r @-` contains precisely the selected paths/hunks and intended message.
- `jj diff` contains the remaining WIP; `jj status` may correctly be nonempty.
- The aggregate final tree and working files/modes match the baseline, except independently identified concurrent edits.
- Bookmarks and public Git refs did not move unexpectedly; no new conflicts appeared.
- In colocated Git, HEAD matches the selected parent and remaining WIP is visible as local changes.

**Do not execute `jj new` afterward. Do not describe, finalize, or move unrelated WIP to a parent change to make `@` empty.** Leave the remaining change's description alone unless the user included it in the request. Stop after checks pass.

If all content is selected, verify the installed version's actual result instead of assuming the partial-split diagram applies; this still does not authorize an extra `jj new`. For several groups, split only the authorized groups and inspect each result. `jj describe` changes a description; it does not isolate content.

## Recovery and Git GUI discrepancies

Recovery is not part of a successful split. Inspect current state and the operation log before proposing or performing it.

- If an unwanted `jj new` is still the latest operation, `jj undo` can undo it while keeping the preceding split. Later operations require identifying the correct operation and its effects; never recommend blind repeated undo.
- An extra `jj new` can temporarily export the WIP as Git HEAD. Undo may restore HEAD while leaving that visit in HEAD's reflog. A Git GUI can also retain a cached row.
- Distinguish jj current/parent changes, Git HEAD, branch/tag refs, `refs/jj/keep/*`, reflog entries, and GUI cache. Inspect what actually reaches the reported commit. Do not claim a specific GUI supports hiding arbitrary refs without verifying it.
- Refresh or reopen the affected repository view and verify the visible result. Do not mistake changing filters for deleting a record, or object retention for failed undo.
- Only when the user authorizes removal of an identified erroneous reflog entry: back up the reflog, verify the exact entry immediately before mutation, and remove only that entry. `git reflog delete --rewrite 'HEAD@{n}'` is a possible targeted operation; never copy an old numeric selector without checking it again. Verify HEAD, index, WIP, and refs remain unchanged, then verify the GUI. Do not expire the whole reflog.
- Never use `git reset --hard`, delete `refs/jj/keep/*`, prune objects, abandon the active WIP, or alter branches merely to remove a GUI row. `git reset` moves/reset state; it does not delete a commit object.

Report the actual operation and observed outcome. If only a reflog record was removed, say so; do not claim the jj WIP object was deleted. Retain a recoverable backup for any targeted cleanup.
