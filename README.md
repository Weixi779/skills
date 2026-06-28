# skills

[![skills.sh](https://skills.sh/b/Weixi779/skills)](https://skills.sh/Weixi779/skills)

Agent skills for iOS and Swift development.

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
npx skills add Weixi779/skills --skill uikit-style-guide -g -a claude-code
npx skills add Weixi779/skills --skill uikit-style-guide -g -a codex
```

## Skills

| Skill | Scope | Status |
| --- | --- | --- |
| [`uikit-style-guide`](./skills/uikit-style-guide/SKILL.md) | Opinionated UIKit + Swift code-style conventions (MARK, setup, didSet, naming) | ✅ available |
| `swiftui-style` | SwiftUI view decomposition, property wrappers, modifier ordering | 🚧 planned |

## Development

This repository uses the standard `skills` CLI layout:

```text
skills/
└── <name>/
    └── SKILL.md
```

For local development, symlink a skill directory so edits stay in sync:

```sh
ln -s "$(pwd)/skills/uikit-style-guide" ~/.claude/skills/uikit-style-guide
ln -s "$(pwd)/skills/uikit-style-guide" ~/.codex/skills/uikit-style-guide
```

The CLI can discover this repository because each skill is stored at
`skills/<name>/SKILL.md`.

## Sources

`uikit-style-guide` is informed by patterns observed in large production
UIKit/Swift codebases — heavy named `MARK` usage, explicit `setupUI`
orchestration phases, and disciplined `didSet` boundaries — plus the official
Swift naming baseline (clarity over brevity). Corpus pinned for provenance:

| Repo | Commit |
| --- | --- |
| signalapp/Signal-iOS | `e275cdb1b1c6014fc02c64ac4e53746bd13d9947` |
| mozilla-mobile/firefox-ios | `88cb5aa426b00bc1cba95303f80cf8f2084c7207` |
| Dimillian/IceCubesApp | `9c05a720597b3ff13de2e241bf58d3fba0863c09` |
| TelegramMessenger/Telegram-iOS | `6e370e06d147b091b07903071cb1b8a22152492d` |

References: [Swift API Design Guidelines](https://www.swift.org/documentation/api-design-guidelines/)
· [Airbnb Swift Style Guide](https://swift.airbnb.tech/)

## License

[MIT](./LICENSE)
