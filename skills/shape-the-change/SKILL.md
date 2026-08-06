---
name: shape-the-change
description: Reconstruct the current software context and shape an actionable change before implementation. Use after scope and ownership are actionable when the user asks to design an implementation, prepare a refactor or integration, understand a difficult bug before changing it, or clarify how an existing system should change. Inspect relevant code, history, tests, and approved decisions; consume the accepted boundary without casually reopening it; choose a local change, scoped reconstruction, or return to boundary discussion; clarify what to keep, remove, replace, migrate, and verify; then wait for explicit user confirmation. Do not use for unresolved product boundaries, pure diagnosis without authorization to design a change, trivial mechanical work, already-confirmed implementation, or naming and style discussions.
---

# Shape the Change

Reconstruct before modifying. Understand why the current system has its present
shape, then make the intended change coherent enough for the user to confirm.

Stay before implementation. Read and investigate freely, but do not modify
product code while this skill is active.

## Consume the Boundary

Find accepted scope in prior conversation, an `argue-the-boundary` handoff, a
project design, ADR, plan, issue, or another approved source. Treat accepted
Requirements, Ownership, Non-goals, and deferred capabilities as inputs; do not
reopen them merely because a more complete design is possible.

Return to boundary discussion and stop when verified facts contradict the
accepted boundary, a reopen condition is met, or shaping the change requires a
new product guarantee, owner, or scope expansion.

Exit immediately for mechanical work, an already-confirmed implementation,
pure naming or style, or diagnosis without authority to design a change.

## Reconstruct Enough Context

Inspect the smallest connected context needed to determine:

- the current call and data flow;
- state and lifecycle owners;
- behavior and compatibility that must remain;
- whether the pressure is local, repeated, or structural;
- why the current structure exists;
- affected consumers, tests, migrations, and old paths.

Use code, tests, history, documentation, runtime evidence, and dependency
sources as needed. Investigate discoverable facts before asking the user, and
separate verified behavior from comments, historical accidents, candidate
explanations, and proposed changes.

## Shape the Change

Resolve only decisions required for a coherent implementation. Ask one
user-owned question at a time, explain why it changes the direction, and give a
recommended answer with evidence and tradeoffs.

Choose one intensity:

- **Local change** — the root cause and ownership are local; preserve the
  surrounding structure.
- **Scoped reconstruction** — repeated failure, duplicated state, or a broken
  dependency direction justifies replacing one coherent path inside the
  accepted boundary.
- **Return to boundary** — Requirements, Ownership, or Non-goals must change
  before implementation can be designed.

Match intensity to verified pressure. Do not prefer either the smallest patch
or the broadest rewrite by default.

## Make the Transition Clear

Use the user's requested carrier without requiring a fixed document or
template. Leave the user able to identify:

- how the relevant system works now and the verified root cause;
- the accepted boundary and assumptions being consumed;
- the proposed coherent change;
- what remains, is removed, is replaced, or must migrate;
- state and lifecycle ownership;
- touched scope, compatibility, and verification;
- risks, non-goals, and open decisions.

Focus on ownership and transition. Do not fabricate implementation code, force
an exhaustive file checklist, or expand into unrelated cleanup.

## Confirm and Stop

Let the user revise the shaped change. Confirmation freezes only the intended
change; it does not authorize implementation. End the skill and wait for a
separate, explicit implementation request.
