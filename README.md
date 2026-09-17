# skills

[![skills.sh](https://skills.sh/b/Weixi779/skills)](https://skills.sh/Weixi779/skills)

Personal agent skills for architecture, code navigation, development workflows,
iOS, and Swift.

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
npx skills add Weixi779/skills --skill uikit-style-principles -g -a codex
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
| [`shape-commits`](./skills/shape-commits/SKILL.md) | Shape task-related changes into intentional atomic commits | ✅ available |
| [`create-pull-request`](./skills/create-pull-request/SKILL.md) | Safely publish scoped changes as an assigned draft GitHub PR | ✅ available |
| [`maintain-pull-request`](./skills/maintain-pull-request/SKILL.md) | Inspect and safely maintain existing GitHub PRs | ✅ available |
| [`uikit-style-principles`](./skills/uikit-style-principles/SKILL.md) | Apply focused UIKit style-change guardrails without imposing a template | ✅ available |
| [`swift-readability`](./skills/swift-readability/SKILL.md) | Improve Swift reading flow with meaningful model operations and shallow functions | ✅ available |
| [`does-it-still-make-sense`](./skills/does-it-still-make-sense/SKILL.md) | Decide whether an evolving codebase still has coherent architecture | ✅ available |
| [`using-codegraph`](./skills/using-codegraph/SKILL.md) | Route structural code exploration through the configured CodeGraph MCP server | ✅ available |
| [`argue-the-boundary`](./skills/argue-the-boundary/SKILL.md) | Pressure-test requirements, scope, and ownership before planning | ✅ available |
| [`shape-the-change`](./skills/shape-the-change/SKILL.md) | Shape an accepted boundary into a coherent implementation direction | ✅ available |
| `swiftui-style` | SwiftUI view decomposition, property wrappers, modifier ordering | 🚧 planned |

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
