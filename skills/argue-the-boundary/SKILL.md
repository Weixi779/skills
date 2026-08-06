---
name: argue-the-boundary
description: Challenge an emerging software proposal before planning or implementation. Use when the user asks to argue or pressure-test a design boundary; when research, redesign, architecture, ownership, or scope decisions are unresolved; when existing code, external practices, AI suggestions, or review findings may be mistaken for requirements; or when review ratchet appears. Separate requirements, constraints, accepted preferences, observations, evidence, proposals, historical accidents, and unknowns; investigate discoverable facts first, question the user one decision at a time, and freeze the current boundary and non-goals. Do not use for already-approved local changes, mechanical implementation, ordinary code review, pure naming or style discussions, or implementation details that do not change scope or ownership.
---

# Argue the Boundary

Argue with the claim, not the user. Determine why a boundary belongs where it
is before allowing a proposal to harden into a requirement, architecture, or
implementation commitment.

Stay before planning and implementation. Read and investigate, but do not edit
product code or turn the result into an implementation plan.

## Calibrate the Argument

Use a full argument when a decision is expensive, difficult to reverse,
architecture-shaping, supported by mixed evidence, or vulnerable to scope
expansion.

For a small, reversible proposal with an established requirement and owner,
inspect the decisive facts, raise at most one unresolved question, and explain
why the boundary is already clear. Exit immediately for naming, local style,
mechanical work, or an already-approved change.

## Build the Claim Ledger

Reconstruct the current product goal, real consumers, existing behavior,
accepted decisions, constraints, and failure evidence. Inspect available code,
history, tests, documentation, runtime evidence, or public sources before
asking the user anything discoverable.

Classify each consequential claim:

- **Observation** — something seen, not yet a conclusion.
- **Evidence** — verified material supporting or weakening a claim.
- **Requirement** — a result the current product must deliver, with an owner
  and a consequence if omitted.
- **Constraint** — an external limit the design cannot negotiate away.
- **Accepted Preference** — a personal or team choice explicitly adopted
  without pretending it is a product necessity.
- **Proposal** — a candidate solution, including AI suggestions, research,
  reviews, generators, and existing abstractions.
- **Historical Accident** — current behavior without confirmed ownership.
- **Unknown** — a material fact or decision that remains unresolved.

Do not promote current code, an external pattern, a review finding, or a
plausible future consumer into a Requirement without current evidence.

## Resolve the Boundary

Walk the highest-impact unresolved branch depth-first:

1. Investigate facts the repository or public sources can answer.
2. Ask one user-owned question at a time.
3. Explain briefly why the answer changes scope or ownership.
4. Recommend an answer with its evidence and tradeoff.
5. Update the Claim Ledger and resolve prerequisites before dependent choices.

Apply only the tests needed for the current claim:

- Who needs this result now, and what fails without it?
- Which layer knows the facts and owns the lifecycle, state, and failure?
- Does an abstraction own a real invariant, consumer, implementation boundary,
  or repeated pressure?
- Which old path, duplicate state, or observed failure would it remove?
- Is research establishing applicability or merely expanding the candidate set?
- Did a correctness review introduce a stronger guarantee than accepted scope?
- Is a tool performing verified mechanics or unverified business interpretation?

Record each material claim as **Accept**, **Reject**, **Defer**, or
**Needs Evidence**. Personal preference is legitimate; name it and let the user
own it rather than deleting it or laundering it into a Requirement.

## Freeze and Stop

Stop when the current result, consumers, ownership, boundary, non-goals, and
reopen conditions are clear enough for a later phase. Use the user's requested
carrier—conversation, project document, planning surface, or another established
form—without imposing a fixed template.

Leave the user able to identify:

- the current product result and consequential claim classifications;
- what belongs inside and outside the boundary, and who owns it;
- accepted, rejected, deferred, and evidence-dependent decisions;
- non-goals and concrete evidence that would justify reopening them;
- what a later phase may assume without arguing again.

User confirmation freezes only the boundary and claim decisions. It does not
authorize change design or implementation. End the skill and wait for a
separate, explicit request.
