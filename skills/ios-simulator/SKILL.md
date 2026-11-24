---
name: ios-simulator
description: 需要操作 iOS 模拟器时使用。选择或启动模拟器，安装和启动 App，模拟设备条件，取截图、录屏或运行日志。交互式 UI 验证可衔接 Xcode 的 device-interaction skill。
---

# iOS Simulator

只需确认一个已有 SwiftUI Preview 的外观时，优先用 Xcode MCP 的 `RenderPreview`。需要运行 App 或改变设备状态时，再选择模拟器。

## 选定设备

从 `xcrun simctl list devices available` 选择符合项目要求的 runtime 和机型，后续命令共用它的 UDID。复用已有目标时，`xcrun simctl bootstatus "$udid" -b` 会按需启动并等到就绪。多台设备运行时，`booted` 可能指向别人的模拟器。

## 执行操作

Xcode 27 优先使用 Device Hub 对应的 `xcrun devicectl`。用 `xcrun devicectl help device <子命令>` 查当前版本的参数。当所选 Xcode 缺少该操作，或目标 runtime 不支持它时，对同一 UDID 使用 `simctl`。

| 操作 | `xcrun devicectl device …` | `xcrun simctl …` 回退 |
| --- | --- | --- |
| 安装模拟器构建的 App | `install app --device "$udid" "$app"` | `install "$udid" "$app"` |
| 启动 App | `process launch --device "$udid" "$bundle_id"` | `launch "$udid" "$bundle_id"` |
| 打开深链 | `process openURL --device "$udid" "$url"` | `openurl "$udid" "$url"` |
| 截图 | `capture screenshot --device "$udid" --destination screen.png` | `io "$udid" screenshot screen.png` |

录屏用 `devicectl device capture screen-record`，回退到 `simctl io "$udid" recordVideo`。Xcode 27.2 模拟多点路线用 `xcrun devicectl device simulate location route --device "$udid" --route-file route.json`，JSON 格式见该命令的 `--help`，结束后用 `location clear` 恢复定位。其他模拟条件查 `xcrun devicectl help device simulate`。

- `push` 向 App 注入推送 payload，可验证 App 的处理逻辑。端到端 APNs 投递需要单独验证。
- `privacy` 调整权限，`get_app_container` 取容器在 Mac 上的路径，`spawn "$udid" log stream` 可按进程或 subsystem 筛日志。
- `create`、`shutdown` 和 `delete` 管理模拟器生命周期。清理本次创建的临时设备，保留原先在用的设备和数据。

## 需要点击、滑动或检查 UI 层级时

项目已有覆盖该操作的 UI 测试时，直接运行该用例。需要交互探索时，使用 Xcode MCP 和 Apple 的 `device-interaction` skill。若 skill 未出现在列表，运行 `xcrun agent plugin path --plugin-format codex`，读取返回目录下的 `skills/device-interaction/SKILL.md`。点按前从当前 MCP 工具列表选动作：27.2 注册的是 `DeviceInteractionSynthesize`，Apple skill 中的 `DeviceEventSynthesize` 是旧名。

如果环境提供的是 Argent MCP，可用已安装的 `argent-ios-simulator-setup` 连接同一台模拟器。
