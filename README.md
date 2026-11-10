# skills

[![skills.sh](https://skills.sh/b/Weixi779/skills)](https://skills.sh/Weixi779/skills)

Personal agent skills for architecture, code navigation, development workflows,
UI previews, documents, iOS, and Swift.

This repository owns portable workflows and their trigger boundaries. Project
facts stay in their project repositories; Weixi's personal engineering judgment
and evidence live in [Petrichor](https://github.com/Weixi779/Petrichor), which an
environment may provide as Memory but no skill requires.

## Install

Install from GitHub with the `skills` CLI:

```sh
npx skills add Weixi779/skills
```

List available skills without installing:

```sh
npx skills add Weixi779/skills --list
```

Install a specific skill globally for an agent:

```sh
npx skills add Weixi779/skills --skill ryugi-visual-docs -g -a codex
npx skills add Weixi779/skills --skill article-poster -g -a codex
npx skills add Weixi779/skills --skill swiftui-style -g -a codex
npx skills add Weixi779/skills --skill uikit-style-principles -g -a codex
npx skills add Weixi779/skills --skill uikit-view-style -g -a codex
npx skills add Weixi779/skills --skill swift-readability -g -a codex
npx skills add Weixi779/skills --skill does-it-still-make-sense -g -a codex
npx skills add Weixi779/skills --skill using-codegraph -g -a codex
npx skills add Weixi779/skills --skill argue-the-boundary -g -a codex
npx skills add Weixi779/skills --skill shape-the-change -g -a codex
npx skills add Weixi779/skills --skill shape-commits -g -a codex
npx skills add Weixi779/skills --skill create-pull-request -g -a codex
npx skills add Weixi779/skills --skill maintain-pull-request -g -a codex
```

## Skills

| Skill | Scope | Status |
| --- | --- | --- |
| [`ryugi-visual-docs`](./skills/ryugi-visual-docs/SKILL.md) | Create UI artboards, interactive previews and commentable documents with the existing Ryugi runtime, without its development workflows | ✅ available |
| [`article-poster`](./skills/article-poster/SKILL.md) | Generate and review article covers against article imagery, site art direction, and responsive banners | ✅ available |
| [`shape-commits`](./skills/shape-commits/SKILL.md) | Shape task-related changes into Git commits or jj changes while preserving WIP | ✅ available |
| [`create-pull-request`](./skills/create-pull-request/SKILL.md) | Publish scoped Git or jj changes as an assigned draft GitHub PR | ✅ available |
| [`maintain-pull-request`](./skills/maintain-pull-request/SKILL.md) | Maintain existing GitHub PRs with Git or jj while preserving WIP | ✅ available |
| [`uikit-style-principles`](./skills/uikit-style-principles/SKILL.md) | Apply focused UIKit style-change guardrails without imposing a template | ✅ available |
| [`uikit-view-style`](./skills/uikit-view-style/SKILL.md) | Apply Weixi's UIKit layout preferences: lazy var + then, view properties last, and setupUI assembly | ✅ available |
| [`swift-readability`](./skills/swift-readability/SKILL.md) | Improve Swift reading flow with meaningful model operations and shallow functions | ✅ available |
| [`does-it-still-make-sense`](./skills/does-it-still-make-sense/SKILL.md) | Decide whether an evolving codebase still has coherent architecture | ✅ available |
| [`using-codegraph`](./skills/using-codegraph/SKILL.md) | Route structural code exploration through the configured CodeGraph MCP server | ✅ available |
| [`argue-the-boundary`](./skills/argue-the-boundary/SKILL.md) | Pressure-test requirements, scope, and ownership before planning | ✅ available |
| [`shape-the-change`](./skills/shape-the-change/SKILL.md) | Shape an accepted boundary into a coherent implementation direction | ✅ available |
| [`swiftui-style`](./skills/swiftui-style/SKILL.md) | Apply Weixi's SwiftUI preferences: scoped child views, direct rendering, and coherent environment ownership | ✅ available |

## Ryugi visual docs

`ryugi-visual-docs` provides focused instructions for UI artboards, interactive
HTML previews, and commentable documents. It uses an existing Ryugi artifacts
runtime; installing this skill does not install that runtime or disable the
original Ryugi plugin and its skills.

The skill does not introduce brainstorming, implementation, TDD, worktree,
review, or commit workflows. Choices and acceptance controls are optional;
showing a design does not authorize implementation.

By default, generated artifacts and runtime state are kept under
`$HOME/Develop/.visual-docs/<project>/`, separate from source repositories.
The runtime creates its `.ryugi/` directory there. An explicit output location
overrides this personal default. Runtime lookup and feedback commands are in
[the runtime reference](./skills/ryugi-visual-docs/references/runtime.md).

## Development

This repository uses the standard `skills` CLI layout:

```text
skills/
└── <name>/
    └── SKILL.md
```

The CLI can discover this repository because each skill is stored at
`skills/<name>/SKILL.md`.

## License

[MIT](./LICENSE)
