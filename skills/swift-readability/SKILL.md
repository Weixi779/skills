---
name: swift-readability
description: Improve existing Swift code when the user asks for readability or local code-quality cleanup, especially long functions, deep nesting, mixed detail levels, or model behavior scattered across callers. Favor meaningful model operations and clear sequential flows while preserving behavior. Do not use for formatter-only work, declaration-only grouping, or unresolved architecture decisions.
---

# Swift Readability

Make call sites read as a sequence of meaningful operations. Prefer short, shallow
functions and behavior beside the state it governs, without imposing a template
or a fixed line-count limit.

## Stay Within the Request

- Read the touched types and relevant callers before moving behavior. Identify
  the state, lifecycle, and observable effects that the current code preserves.
- Match the requested action: discuss ideas when asked for ideas; perform an
  authorized cleanup without asking for the same authorization again. A request
  for `MARK` or extension grouping alone does not authorize model reconstruction.
- Keep existing API and behavior stable. Do not bundle bug fixes, new product
  policy, or broader architecture changes into a readability refactor.

## Give Models Complete Operations

- Look for callers repeatedly comparing identity or compatibility, coordinating
  several fields, or keeping related collections in sync. Put the complete
  operation on the existing model that owns those facts and state.
- Make the operation maintain its invariant so callers no longer manipulate the
  same fields and repeat the same rules. Keep meaningful differences between
  model types instead of unifying them through flags or erasure.
- Inspect construction and consumers before tightening state. Required values
  can be established at initialization; a query needing only a presenter and its
  context should not invent a lifecycle token to fit a larger return type.
- Confirm value versus reference semantics before changing mutation or writeback.
  A model may remain plain data; do not invent a Manager, protocol, or generic
  framework merely to move code out of a long function.

## Keep the Reading Flow Continuous

- Keep a function's steps at a similar level of detail. Move a nested lookup,
  compatibility rule, or state transition behind a name that explains its result.
- Extract a coherent operation, not an arbitrary block of lines. Prefer distinct
  actions such as preparing display and refreshing behavior over a universal
  `update` with several Boolean switches.
- Reduce nesting with explicit prerequisites and local operations. An early
  return must exit only the intended step: a failed optional refresh may still
  need to be followed by bookkeeping or an event callback.
- Use `private extension` and `MARK` where they improve navigation. Do not enforce
  a fixed member order, section list, function length, or helper count. Judge the
  result by what the reader must keep in mind and how often they must jump around.

## Make Call Sites Natural and Honest

- Prefer Swift and platform facilities when they express the operation clearly:
  default-parameter initializers for configuration, typed values, and ordinary
  mutation before introducing a custom DSL. Keep an established local API when
  changing it adds no concrete readability benefit.
- Let names use the context already supplied by their type or module. Distinguish
  creation, configuration, conversion, and parsing instead of calling everything
  `make`. Use predicates such as `has`, `can`, and `should` when their meaning fits;
  these are cues, not a mandatory naming dictionary.
- Keep mutation and effects visible. Do not hide coordination, caching, or
  read-modify-write semantics in clever getters and setters. Use transformations
  and value-semantic chaining when they clarify the result; use direct statements
  for effects rather than side-effectful `map` or unnecessary copy-and-writeback.
- Preserve useful public type relationships. Internal type erasure does not
  justify replacing a caller's generic constraints with `Any`; shorter code must
  not quietly weaken the contract or expose internal machinery.

## Preserve and Verify the Behavior

- Preserve evaluation and side-effect order, identity rules, lifecycle phases,
  recovery behavior, and the state snapshot used by callbacks under reentrancy.
  Keep moved behavior on the same actor and preserve selector identity.
- Review the complete diff for unintended behavior changes. Run checks suited to
  the moved logic; add focused regressions for state transitions or callback
  sequences when existing coverage is insufficient.
- Report the operations moved, the resulting call-site shape, and verification
  limits. Fewer lines and successful parsing alone do not establish correctness.
