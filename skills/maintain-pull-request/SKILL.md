---
name: maintain-pull-request
description: Inspect and maintain an existing GitHub pull request when the user asks to review or address feedback, fix checks, rebase or update the branch, reply to or resolve comments, or update pull request metadata.
---

# Maintain Pull Request

1. Read the repository instructions and identify the target pull request.
2. Inspect the complete pull request diff, description, commits, checks, and thread-aware review state.
3. Judge every comment against the actual code flow and product requirements. Do not assume reviewer suggestions are correct.
4. Respect the requested scope:
   - Keep inspection requests read-only.
   - Modify and validate locally for fix requests without publishing unless requested.
   - Commit, push, reply, and resolve only when explicitly requested.
5. Preserve unrelated changes and verify the final `base...HEAD` diff before publishing.
6. After a rebase, use `--force-with-lease`; never use an unconditional force push.
7. Reply to each handled thread with the concrete outcome. Resolve it only after the fix is pushed or a no-change conclusion is explained.
8. Respond at pull request level when feedback has no resolvable thread.
9. When publishing later requirements that invalidate an earlier reply or fix, explain the correction in the affected thread.
10. Preserve the current Draft or Ready state unless explicitly asked to change it. Never automatically mark a pull request ready.
11. Report commits, pushes, replies, resolutions, checks, and remaining issues accurately.
