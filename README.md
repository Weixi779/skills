# skills

Personal [Claude Code](https://claude.com/claude-code) / agent skills by
[@Weixi779](https://github.com/Weixi779), focused on iOS / Swift.

## Skills

| Skill | Scope | Status |
| --- | --- | --- |
| [`uikit-style-guide`](./skills/uikit-style-guide/SKILL.md) | Opinionated UIKit + Swift code-style conventions (MARK, setup, didSet, naming) | ✅ available |
| `swiftui-style` | SwiftUI view decomposition, property wrappers, modifier ordering | 🚧 planned |

## Install

Skills live under `skills/<name>/SKILL.md` so the `skills` CLI can discover
them from GitHub:

```sh
npx skills add Weixi779/skills
```

For local development, symlink the skill directory so it stays in sync with this
repo:

```sh
ln -s "$(pwd)/skills/uikit-style-guide" ~/.claude/skills/uikit-style-guide
```

Verify it loaded by listing skills in Claude Code, or just start writing UIKit
code and let it trigger.

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
