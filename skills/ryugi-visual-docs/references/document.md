# 可批注文档

集中归档项目的 `.ryugi/artifacts/document/<topic>/doc.xml` 使用 LiteXML，不是 Markdown。图片放 `assets/`，相对引用。

```xml
<doc>
  <h1>评论依赖设计</h1>
  <p>本文说明当前消费者需要什么，以及组装点如何提供这些能力。</p>
  <h2>结论</h2>
  <p>这里写本轮结论，并区分已确认决定与建议。</p>
  <codeblock lang="swift"><![CDATA[struct Example {}]]></codeblock>
  <mermaid><![CDATA[flowchart LR
    Consumer --> Dependency
    Dependency --> Container]]></mermaid>
  <details summary="依据"><p>实际调用处与验证结果。</p></details>
</doc>
```

常用块：h1–h6、p、ul/ol/li、table/tr/th/td、blockquote、codeblock、mermaid、details、img；行内 b/i/code/a。代码和 Mermaid 用 CDATA 避免转义问题。图表仅在有真实数值时使用；不要为了排版增加无必要的块。格式正确性以运行时 check 为准。

首屏说清主题和结论或待讨论问题。正文按读者理解组织，不规定固定章节或问题数量。尚在写的段落可用 `<pending intent="待补充内容"/>`，交付完成稿前移除。

## 保留共同编辑

每次编辑前重新读 doc.xml：用户和服务端也会写入。服务端会规范标签和空白、添加 block id。保留已有 id，只改目标段落，不用旧副本覆盖整篇；新块不手造 id。出现 merging 状态时再 check，不声称同步已完成。

用户要求看其修改时用 `changes --project-dir ... --artifact document/<topic>`，按打印的 Read 读取完整 diff，再用对应 receipt 确认。批注使用 runtime.md 中的 CLI，不直接编辑 comments.json。

## 引用其他产物

```xml
<p><ref artboard="comment-layout" group="layout" /></p>
<p><ref html="menu-prototype">体验菜单</ref></p>
```

每个 ref 只指定一种目标类型；artboard 可选一个 group 或 board。引用必须位于 p/li/表格单元格等行内容器中。服务端添加的 to 属性保持不动。被引用 ID 改名时同步更新引用，并解决 check 的目标缺失警告。

## 可选问题

需要文档内提交答案时才使用：

```xml
<p>密度选择：<ask id="density" recommend="roomy"><option value="compact">紧凑</option><option value="roomy">宽松</option></ask></p>
```

问题 ID 稳定且唯一，新问题用新 ID。`comment list --section asks --detail` 读取状态；只有 submitted 才是提交结果。recommend 可能成为用户明确提交时未改动的默认值，不能把未提交、无回复当成答案。

按实际答案处理当前文档后，通过 `comment note ... --ask ID`（正文 stdin）说明结果。没有答案时如实记录仍未确定，不自行补成同意。撤回的提交不继续处理。不要求每份文档必须有问题或得到批准才结束。
