---
name: uikit-style-principles
description: Apply focused principles when the user explicitly asks to reorganize, clean up, or review the style of existing UIKit Swift code. Preserve behavior, API, lifecycle, Objective-C selector identity, and side-effect ordering while keeping the diff proportionate. Do not use for ordinary UIKit feature implementation, bug fixing, API guidance, formatter- or lint-only work, SwiftUI, or unresolved architecture decisions.
---

# UIKit Style Principles

Improve reading navigation and local organization without changing semantics.
This skill defines an edit boundary; it is not a UIKit tutorial, architecture
policy, modernization mandate, or fixed file template.

## Establish the Contract

- Follow the explicit task, UIKit and Swift contracts, repository instructions,
  and verified behavior before applying these principles.
- Read the whole touched type and only as much nearby code as needed to
  understand its local organization and behavioral boundaries.
- Treat style-only work as behavior- and API-preserving. If a useful change
  crosses that boundary, leave it unchanged and surface it as a separate
  proposal requiring its own authorization.

## Keep the Diff Focused

- Change only the requested scope and adjacent code required to keep that scope
  coherent. Do not normalize the whole file or touched type unless that is the
  scope the user explicitly requested.
- Do not modernize APIs, fix business wording, rename unrelated symbols, or
  clean up apparently redundant logic along the way.
- Group code by real responsibility. Add a `MARK` or extension only when it
  materially reduces navigation cost for the changed responsibility. Extract a
  helper only when the task explicitly authorizes it and the extraction does
  the same.
- Do not impose a universal member order, `MARK` set, conformance placement,
  setup phases, naming vocabulary, or selector-versus-`UIAction` choice.
- Preserve a coherent local choice when multiple UIKit forms are equally valid.

## Read the Type by UIKit Role

Use these as responsibility lenses, not mandatory member orders:

- In a view controller, keep its external API, dependencies, owned state and
  views, initialization, lifecycle, assembly, bindings, state presentation,
  actions, and routing outputs discoverable. Leave timing-dependent work in the
  lifecycle callback whose semantics it requires.
- In a view or control, distinguish public inputs and events, internal state and
  subviews, one-time assembly, changing-state presentation, geometry, and event
  handling. Preserve standard `UIControl` event semantics at its public boundary.
- In a cell or reusable view, distinguish content identity, stable construction,
  complete configuration, residual reuse cleanup, events, and late callbacks.
  A display cycle must not accidentally depend on the previous represented item.
- Keep a short core conformance with the type when that aids discovery. Use an
  extension or collaborator only when it exposes a distinct responsibility,
  lifetime, or test seam rather than satisfying a line-count rule.

## Use Vocabulary by Responsibility

Use a name only when the operation has that meaning; do not rename equivalent
local vocabulary merely to conform to this list.

- `setupUI` orchestrates meaningful one-time assembly; do not create empty setup
  phases or put changing model state there.
- `bind` establishes a long-lived event or state connection whose owner and
  lifetime remain visible.
- `configure` accepts external input and establishes the current content identity
  without rebuilding stable structure or accumulating connections.
- `render` projects a complete state repeatably; `update...` or `apply...` names
  the intentionally narrower state or component it changes.

## Keep Ownership and Reuse Visible

- Keep tasks, observers, subscriptions, callbacks, delegates, and collaborators
  with the owner of their useful lifetime. Choose capture and cancellation from
  the actual retain and lifetime graph, not from a universal weak-or-cancel rule.
- Choose closure, target-action, delegate, or collaborator from the interaction's
  semantics and coherent local practice, not as an incidental modernization.
- When organizing or reviewing a reusable view, keep complete configuration,
  residual reuse cleanup, and late-callback identity visibly distinct. Do not
  change those behaviors under a style-only scope.

## Preserve Exact Semantics

- Do not add `final`, narrow access, alter initializer or decoder paths, change
  override or conformance dispatch, change actor isolation, or alter directional
  layout meaning as part of style work.
- Do not delete repeated branches, rewrite expressions or conditions, change
  value sources, or fix business typos. Private code is still behavior.
- Move complete declarations or method blocks rather than rewriting statements.
  Do not extract helpers by default; when explicitly authorized, preserve the
  exact statement sequence, evaluation count, captures, lifetime, and error
  behavior.
- Keep work in its existing UIKit lifecycle or reuse phase. Preserve the stage
  and relative order of targets, delegates, data sources, callbacks, observers,
  subscriptions, bindings, tasks, fetches, child containment, and other side
  effects.
- Treat Objective-C selector identity as runtime API even for a `private`
  action. Do not rename an `@objc` method unless the task explicitly authorizes
  an API migration and its selector contract is handled deliberately.

## Check the Result

- Re-read the diff for changes to symbols, API visibility, selector identity,
  lifecycle placement, statement sequence, side-effect order, and layout
  semantics.
- Remove churn outside the authorized scope. Let project formatter and lint
  configuration own mechanical style.
- Run project-required validation in proportion to the risk. Swift parsing,
  formatting, or a clean diff alone does not prove behavior was preserved.
