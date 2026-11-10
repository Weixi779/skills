# UI 画板

文件在集中归档项目的 `.ryugi/artifacts/artboard/<topic>/`：`canvas.json` 描述布局，`pages/` 放 HTML 和媒体。每个 HTML board 是独立 iframe。

最小例子：

```json
{
  "version": 1,
  "title": "评论布局比较",
  "activePage": "main",
  "pages": [{"id": "main", "name": "页面方案"}],
  "boards": {
    "compact": {"file": "pages/main/compact.html", "page": "main", "x": 0, "y": 0, "w": 390, "h": 844, "title": "紧凑布局"},
    "roomy": {"file": "pages/main/roomy.html", "page": "main", "x": 470, "y": 0, "w": 390, "h": 844, "title": "宽松布局"}
  },
  "groups": {"layout": {"title": "间距比较", "boards": ["compact", "roomy"]}}
}
```

title 使用可读名称，不等于目录 slug。先写 manifest，再写完整 HTML；以 `</html>` 结束，缺失或未完成页面会保持 pending。HTML 内联样式和逻辑，`html, body` 清除默认 margin；所有本地资源必须在 `pages/` 内，相对引用。不要引用源码目录的绝对路径。

## 排列与编辑

board 以 x/y 定位，w/h 为 iframe 尺寸；内容超高会被裁切，必须自行检查。常用手机 390×844、桌面 1280×800，按内容调整。横排间隔 80px；组边框在顶部留 144px、其余侧留 48px；上下组的 board 留 240px，避免标签相撞。

已有画板先读 `.ryugi/artifacts/.state/artboard/<topic>/layout.json` 获取实际 bounds。稳定保留被文档引用的 board/group ID。局部修改只改对应 HTML，不重建整个画板。新方案可以放在现有内容旁或下方，不强制开启新轮次。

截图 board 用 `type: "image"` 和 `src` 替代 file，视频用 `type: "video"`，资源同样放 pages 下；单文件限制 8MB，总画板 20MB。视频要保留原始时间证据时设置 `trim: false`。

用户指着元素说“这里”时，读取 `.state/artboard/<topic>/selection.json` 的 selected/tweaks；需要节点 ID 时才查 nodes。Inspector 改动只是预览，必须修改源文件才会保存。刷新后节点编号可能改变，重新定位，不复用旧编号盲改。过期批注的 CSS path 也要对照当前 HTML。

## 可操作原型

需要实际点击的 board 增加 `interactive: true`；静态方案不加。用原生按钮、事件处理和 board 内状态实现动作，相关流程放在同一个 iframe。跨 board 链接不会共享状态，也不会切换画布选中的 board。

沙箱不能使用 localStorage、sessionStorage 或 cookies。刷新、Replay、切换画布页、展开独立视图可能重建状态。验证首次打开、连续操作、关闭再打开、空状态和滚动到底；没有实现的操作明确标示，不用空按钮冒充可交互。

## 可选选择与验收

确实要用户在画板选项时，group 加 `choose: true` 和 `recommend: "board-id"`；明确要求验收才加 `verify: true`，不能同时使用。

选择读取：`comment list --sections threads,groups,rounds --detail`。只处理 submitted 轮次；同时读完批注。口头明确提交选择时可用 `comment choose ... --group ID --board ID` 和 `comment submit ... --round ID` 记录。没有回复或没有明确提交时，不替用户提交；运行时可能给未选项套默认推荐，不能将此扩展为工程授权。

验收读取：`comment list --sections groups,threads --detail`。只有用户给出通过或拒绝结论，才用 `comment pass` 或 `comment reject` 代录。修改证据会使验收返回 pending；拒绝线程不自行 resolve。普通展示无需这些控件，也不以全部通过作为任务默认完成条件。
