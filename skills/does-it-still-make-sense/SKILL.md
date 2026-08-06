---
name: does-it-still-make-sense
description: >
  Assess whether an evolving codebase's current architecture still reflects
  real intent, ownership, and change pressure. Use only when the user explicitly
  invokes `$does-it-still-make-sense` or directly asks to run this named review;
  inspect the current code, tests, documentation, and relevant history, then
  return an evidence-backed Markdown verdict that may conclude no change is
  justified. Use Mermaid only when it materially clarifies relationships. Do
  not trigger for ordinary implementation or review, and do not implement the
  proposed changes.
---

# Does It Still Make Sense?

Treat this as a deliberate architectural pause, not a standing cleanup job.
Answer the question before proposing work. A clean bill of health is a successful
outcome, and there is no candidate quota.

## Boundaries

- Stay read-only. Do not edit source, tests, documentation, or decision records.
- Respect the user's scope. If they name a subsystem or pressure point, inspect
  that instead of widening to the whole repository.
- Do not treat file length, file count, protocol count, layers, or unfamiliarity
  as architectural defects.
- Ignore formatting, lint, naming trivia, and ordinary dead code unless they
  expose a deeper ownership or responsibility problem.
- Treat AI navigability as a consequence of coherent design, not the design goal.
- Read tests as architectural evidence, but do not run a full build or test suite
  unless a finding depends on current behavior and the cost is proportionate.
- Return Markdown in the conversation. Do not generate HTML or write a report
  file unless the user explicitly asks for one.

## 1. Choose Where to Look

Read repository instructions first. If the user did not name a scope, use
relevant history to find pressure rather than scanning everything uniformly:

- repeated changes or fixes in the same flow;
- files that repeatedly change together;
- parallel implementations, compatibility paths, or unfinished migrations;
- tests that need broad setup or know internal sequencing;
- concepts whose behavior is spread across several apparent owners.

Read architecture notes and decision records when present, but verify that the
current code still reflects them. A document is evidence of an accepted decision,
not proof that the implementation still matches it.

Stop exploring once representative flows, their owners, relevant history, and
the evidence for a verdict are clear. Do not treat exhaustive repository coverage
as a sign of rigor.

## 2. Reconstruct the Current Story

Start with a real consumer, public entry point, or user-visible intent. Trace a
complete representative flow through interpretation, state, execution, failure,
and output. Identify:

- what the caller is trying to accomplish;
- who interprets the request;
- who owns the complete execution;
- who owns mutable or feedback-driven state and for how long;
- which boundaries are public, replaceable, or merely internal organization;
- which old and new paths still coexist.

Use structural code tools when available. Read focused history for the relevant
types and commits to distinguish deliberate boundaries from transitional shapes.
Do not reconstruct the architecture from filenames alone.

Keep an internal claim ledger while investigating:

- **Requirement** — behavior the system must preserve.
- **Constraint** — an accepted limitation or decision.
- **Observation** — what the current code does.
- **Evidence** — source, test, history, or measured change pressure.
- **Proposal** — a possible future shape.
- **Historical accident** — a shape retained after its original reason changed.
- **Unknown** — something not yet supported strongly enough to classify.

A current type, former capability, external pattern, or plausible idea is not by
itself a requirement.

## 3. Test Whether the Shape Still Earns Its Keep

Apply these questions only to the reconstructed flow:

- Can the flow be explained through clear owners, or must one concept be pieced
  together across many shallow modules?
- Does each abstraction hide meaningful decisions behind a simpler interface,
  or only rename, forward, or relocate them?
- Does state live with the responsibility and lifetime that change it?
- Did a newer structure replace the former owner and path, or merely join them?
- Are correlated changes local to one owner, or repeatedly scattered across seams?
- Can tests exercise the meaningful behavior through its real interface, or do
  they cover extracted pieces while orchestration remains implicit?
- Is a public or replaceable boundary supported by a real consumer, invariant,
  lifecycle, failure mode, or alternate implementation?
- What counterevidence makes the current complexity necessary—for example
  performance, platform constraints, compatibility, or an accepted product boundary?

Apply the deletion test to suspicious abstractions: imagine folding the type into
the responsibility that appears to own the work. If concepts or branches disappear
and the flow becomes easier to explain, the abstraction may be accidental. If
complexity leaks into multiple callers or a stable responsibility loses its home,
the abstraction is earning its existence.

Do not recommend a change unless it has current evidence, a concrete cost, a more
credible owner or boundary, and an account of what the change would remove or
replace. Otherwise record an unknown or leave it alone.

## 4. Form the Verdict

Choose the narrowest defensible verdict:

- **Still makes sense** — the architecture explains the current system; any
  friction is local or cheaper than changing it.
- **Still makes sense, with pressure** — the main shape remains coherent, but
  one or more evidence-backed tensions deserve attention.
- **No longer makes sense** — ownership, state, execution, or boundaries now
  contradict the system's actual intent strongly enough to justify change.

State confidence and the strongest counterargument. Do not inflate the verdict
because a larger report looks more useful.

## 5. Present the Review

Use this Markdown structure, omitting empty sections:

1. **Verdict** — answer in the first paragraph.
2. **Scope and evidence** — what was inspected and why.
3. **The current story** — caller intent, execution owner, state owner, and
   important boundaries.
4. **What still works** — responsibilities and constraints that should remain.
5. **Architectural pressure** — only evidence-backed tensions.
6. **Recommendation** — the one highest-leverage next decision, or an explicit
   recommendation to leave the codebase alone.

For each pressure point include:

- concrete files, symbols, tests, or commits;
- the present owner and consumer;
- what no longer explains itself;
- the deletion or replacement outcome;
- what must remain and what would disappear;
- counterevidence, unknowns, and confidence.

For any reviewed scope with at least three meaningful components, owners, or
transitions, include one Mermaid diagram of the current shape. For a smaller
scope, use Mermaid only when it materially improves the explanation. Show the
current truth first. Add a proposed shape only when evidence supports it. Keep
diagrams small, use domain language with code identifiers where useful, quote
labels that contain punctuation, and avoid decorative diagrams.

End after the verdict and recommendation. If the user chooses a pressure point,
discuss that decision separately and wait for explicit authorization before
planning or implementation.
