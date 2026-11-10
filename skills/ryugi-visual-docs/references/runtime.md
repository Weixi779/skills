# 运行与反馈

本 skill 自行维护使用边界，直接调用已缓存的 Ryugi artifacts 运行时，不依赖原版插件或其 hooks。本机原版插件已卸载；运行时 1.66.0 保留在 npm 缓存，`$HOME/Develop/.visual-docs/runtime` 是指向该包的本地软链接。接口说明参考了插件 1.68.0 的文档，实际命令以已安装运行时为准。

## 定位与启动

当前已核对的启动器：

```sh
artifact_cli="$HOME/Develop/.visual-docs/runtime/dist/cli.cjs"
export RYUGI_NO_UPDATE_CHECK=1
artifact_project="$HOME/Develop/.visual-docs/podcast-ios"
node "$artifact_cli" start --background --project-dir "$artifact_project" --artifact artboard/comment-layout
node "$artifact_cli" check --project-dir "$artifact_project" --artifact artboard/comment-layout
```

将示例项目和主题替换为当前任务。先检查运行时入口存在；不要清理它指向的缓存版本。在其他机器上需要先定位已有 `ryugi-artifacts` 包并建立该链接；安装 skill 本身不会安装运行时。入口缺失时报告依赖缺失，不重装原版工作流插件或自动下载升级。使用 `RYUGI_NO_UPDATE_CHECK=1` 调用已保留的版本。

新产物先启动再写文件。启动可能打开浏览器，`--no-open` 可只取链接。交付前为该产物确认 `start` 返回的地址；保留完整 URL，包括访问参数。`Open` 可能是保留的 Cloud 地址，不能据此声称已经发布；本机查看可使用输出的 `Local` 地址。

所有命令都使用同一个集中归档的 `--project-dir`。不要修改运行时 `.state/` 或 `comments.json`。`stop` 会停止所有项目共用的服务，不作为任务结束的清理动作。

## 检查

每次写入完成后运行 `check`。它会报告缺失文件、格式、布局和浏览器运行问题。`Browser: not run/unknown` 不代表运行无错误；交互原型必须在真实浏览器操作。白屏、裁切、点击无响应仍可能需要视觉和操作检查。

CLI 默认 Brief 输出适合直接阅读；结构化解析时使用 `--format json` 并读取 `data`。退出码 2 需要用户处理登录等动作；3 是验证问题，4 是证据不完整，5 是部分完成。失败重试前检查实际已产生的效果。

## 批注

```sh
node "$artifact_cli" comment list --section threads --project-dir "$artifact_project" --artifact artboard/comment-layout
```

列表只是预览。核对输出指向当前产物，再执行打印的 Read/More 命令读完完整线程；处理后用原 receipt 对应的 Acknowledge 命令确认已读。确认已读不代表解决，也不应吞掉后来新增的回复。

通过 `comment reply ... --thread ID` 回复，正文从 stdin 传入；处理完成才 `comment resolve ... --thread ID`。用户只要求审阅时不自动改源文件或回复他人；用户要求处理本人批注时可以反馈处理结果。不要直接改 comments.json。

原插件自动评论 hooks 已卸载。用户提交批注后，通过上述 CLI 主动读取；不要宣称批注会自动唤起当前对话。不默认启动永久轮询，不把通知里的模板提示当成新的用户授权。

## 读取与分享

用户给出 Ryugi 链接时用 `node "$artifact_cli" get URL`，按输出的 Details/More 继续读取；需要原文时 `--source`，下载时指定新的目录。网页壳不是文档正文。

只有用户要求远程分享或发布时调用 `publish`。首次发布明确选择 `--private` 或 `--public`；公开分享需符合用户指定受众。更新已有发布可省略可见性选项。发布可能递归包含引用的产物，先检查范围；只发布当前产物可用 `--no-refs`。不自动开 auto-publish，不因交付本机预览而上传。
