---
name: uikit-style-guide
description: >
  Review or write UIKit Swift code using an opinionated organization and naming
  style for UIViewController, UIView, custom controls, MARK ordering, setupUI,
  binding, render/update, didSet boundaries, private names, outlets, actions,
  selectors, and protocol-conformance extensions. Do not use for SwiftUI.
---

# UIKit Style Guide

Opinionated UIKit + Swift style. Optimize for:

1. Xcode minimap / Jump to Definition.
2. Local extensibility.
3. Brevity last.

This is a personal convention, not an industry standard.

Scope: UIKit only. Do not apply to SwiftUI.

## Review Behavior

When reviewing UIKit code:

- Prioritize correctness and readability over mechanical style.
- Report in this order: lifecycle/threading correctness → topology/MARK →
  setup/render/didSet boundaries → naming/access control → formatting.
- Use severity labels: `Must fix`, `Should fix`, `Optional`.
- Prefer concrete rewrites over abstract comments.
- Do not flag harmless deviations.
- If local project conventions conflict, prefer consistency within the touched
  file or module.

## Non-Goals

- Do not rewrite working code only for taste.
- Do not extract solely because a file is long; name the responsibility to move.
- Do not create MARK sections for tiny groups without navigational value.
- Do not move workflows into `didSet`.
- Do not apply this guide to SwiftUI.

## File Topology

Use a fixed section order. Always use `// MARK: - Section`.

This guide intentionally places `Views` and `Actions` last so behavior is read
before view construction.

Inside the primary type:

`Types` → `UX`/`Layout`/`Constants` → `Properties` → `Init` → `Lifecycle` →
`Setup` → `Binding` → `Render`/`Update` → `Navigation` → `Private` → `Views` →
`Actions`.

After the primary type, put protocol-conformance extensions under
`// MARK: - ProtocolName`.

Omit empty sections; never reorder the sections you keep.

```swift
final class ProfileViewController: UIViewController {
    // MARK: - Types
    struct State: Equatable {
        let headline: String
    }

    // MARK: - UX
    private enum UX {
        static let spacing: CGFloat = 16
    }

    // MARK: - Properties
    private var state: State

    // MARK: - Init
    init(state: State) {
        self.state = state
        super.init(nibName: nil, bundle: nil)
    }

    @available(*, unavailable)
    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }

    // MARK: - Lifecycle
    override func viewDidLoad() {
        super.viewDidLoad()
        setupUI()
        bind()
        render(state)
    }

    // MARK: - Setup
    private func setupUI() {
        setupAppearance()
        setupViewHierarchy()
        setupConstraints()
        setupActions()
    }

    private func setupAppearance() {}
    private func setupViewHierarchy() {}
    private func setupConstraints() {}
    private func setupActions() {
        saveButton.addTarget(self, action: #selector(didTapSaveButton), for: .touchUpInside)
    }

    // MARK: - Binding
    private func bind() {}

    // MARK: - Render
    private func render(_ state: State) {
        headlineLabel.text = state.headline
    }

    // MARK: - Private
    private func reportStateChangeIfNeeded(_ state: State) {}

    // MARK: - Views
    private let headlineLabel: UILabel = {
        let label = UILabel()
        label.numberOfLines = 0
        label.adjustsFontForContentSizeCategory = true
        return label
    }()

    private let saveButton = UIButton(type: .system)

    // MARK: - Actions
    @objc
    private func didTapSaveButton() {}
}

// MARK: - UITableViewDelegate

extension ProfileViewController: UITableViewDelegate {}
```

## MARK Rules

- Use `// MARK: - Section` for major landmarks only.
- Avoid unnamed `// MARK: -`.
- Avoid ad-hoc MARKs for fewer than ~3 members.
- Canonical sections may exist with one meaningful member.
- Put each protocol conformance in its own `// MARK: - ProtocolName` extension,
  unless the conformance is tiny and local convention groups it.

## Constants, UX, and Layout

- Prefer `UX` for UIKit visual constants.
- Use `Layout` only for layout-heavy views with geometric constants.
- Use `Constants` only for non-visual static values.

## Init Rules

- Prefer dependency injection through `init`.
- For programmatic view controllers, mark storyboard initialization unavailable
  when unsupported.
- For `UIView`s, call shared setup from both initializers only when the view
  supports both programmatic and nib/storyboard usage.

```swift
@available(*, unavailable)
required init?(coder: NSCoder) {
    fatalError("init(coder:) has not been implemented")
}
```

## View Property Rules

- Put stored UIKit view instances under `Views`.
- Prefer `private let` for views that do not need `self` during construction.
- Use `private lazy var` only when construction genuinely needs `self`.
- Keep view factory closures local and side-effect-free.
- Do not start network work, persistence, analytics, or navigation from view
  property initialization.
- Prefer installing target-actions in `setupActions()` rather than in view
  property closures.

## Setup Rules

- Keep `setupUI()` as an orchestration point: no branching, almost no detailed
  UI code.
- Prefer `setupViewHierarchy()` over `setupSubviews()`, especially for nested
  structure.
- Prefer plural `setupGestures()` / `setupActions()` when installing more than
  one recognizer or target.
- When using programmatic Auto Layout, disable
  `translatesAutoresizingMaskIntoConstraints` before activating constraints.

## Binding Rules

- Use `bind()` / `bindViewModel()` for reactive wiring, not `setupUI()`.
- Binding covers delegate/closure callbacks, notifications, Combine, RxSwift,
  async streams, and view-model callbacks.
- Structural delegate/`dataSource` assignment may live in setup; state-producing
  callbacks belong in binding.
- Avoid retain cycles in bindings, notifications, timers, and view-model
  callbacks.

## Render Rules

- Keep `render(_:)`, `apply(_:)`, and `update(with:)` repeatable and idempotent.
- Good render work: text, image, visibility, enabled/selected state,
  configuration.
- Do not add subviews, activate constraints, install actions, start async work,
  navigate, log analytics, or persist data from render.

## didSet Rules

Allow `didSet` only when all of these hold:

- local to the object;
- synchronous;
- idempotent, or guarded with `oldValue`;
- no async work;
- no navigation, analytics, DB writes, network calls, or cross-feature business
  decisions;
- trivial to delete if the property later becomes computed or state-driven.

When in doubt, prefer an explicit method over `didSet`.

```swift
// GOOD — local, synchronous UI forwarding
var headline: String? {
    didSet { headlineLabel.text = headline }
}

// GOOD — workflow lives in a named method
private var state = State()

func update(with state: State) {
    guard self.state != state else { return }
    self.state = state
    render(state)
    reportStateChangeIfNeeded(state)
}

// BAD — workflow hidden in a setter
var isVisible: Bool = false {
    didSet {
        analytics.log(.visibility(isVisible))
        Task { await store.save(isVisible) }
    }
}
```

## Naming Rules

- Public/internal names optimize for the caller.
- Private names may be shorter, but not cryptic.
- No Objective-C-style module prefixes on private members:
  `titleLabel`, not `profileHeaderTitleLabel` inside `ProfileHeaderView`.
- Booleans: `is` / `has` / `can` / `should`.
- Event handlers: past tense, e.g. `didTapSaveButton`, `keyboardWillShow`.
- Acronyms: `URL`, `ID` mid-name; `url`, `id` at a lowerCamelCase start.
- `vc` / `vm` are fine in short local scopes; use clearer names for stored
  properties.
- Avoid empty names like `data`, `info`, `tmp` outside tiny local scopes.

## Interface Builder Rules

For storyboard/nib-based files:

- Put `@IBOutlet` properties under `Views`.
- Keep outlets `private` unless wider access is required.
- Put `@IBAction` methods under `Actions`.
- Put `awakeFromNib()` / `prepareForInterfaceBuilder()` under `Lifecycle`.
- Keep storyboard wiring, appearance, constraints, and actions distinct.

## Additional Rules

- Default UIKit classes to `final` unless designed for subclassing.
- UIKit mutations must happen on the main thread / main actor.
- Prefer dependency injection in `init`; reserve singletons for composition
  roots or legacy boundaries.
- Keep selectors `private` and under `Actions`.
- Give `Accessibility` and `Theme` their own sections once each has more than a
  couple of lines.
- When a file passes ~400–600 lines or needs more than ~8–10 meaningful `MARK`
  sections, look for an extractable responsibility before recommending
  extraction.
