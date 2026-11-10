# 单页交互预览

在集中归档项目的 `.ryugi/artifacts/html/<topic>/index.html` 写完整 HTML，标题用可读名称，资源放同目录并相对引用。使用 HTML/CSS/JS 或当前项目已有的输出，不为单页展示引入新的应用框架。

预览在沙箱运行。交互可以使用原生 DOM、事件与脚本变量；不要使用 localStorage、sessionStorage 和 cookies。input/select/textarea 使用稳定 id，监听 input/change 后从当前控件值渲染：运行时恢复控件值时会触发这些事件。仅放在脚本变量中的状态不能依赖跨重载保留。

给每个可见操作实际结果，检查输入、切换、弹窗、键盘关闭、窄屏与滚动；无需模拟本次无关的完整业务后端。多套方案可用不同 html 产物，或放入 artboard 比较。

用 check 查看当前版本的 runtime 错误。未在浏览器加载时没有报错不代表通过；交付时说明实际运行和验证范围。
