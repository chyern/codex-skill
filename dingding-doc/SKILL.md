---
name: dingding-doc
description: Read, generate, edit, format, and operate DingTalk/Alibaba Docs (钉钉文档/阿里文档) in the web editor. Use when the user asks Codex to read or summarize a DingTalk document, quickly modify an alidocs.dingtalk.com page, insert or update tables, insert or update flowcharts/sequence diagrams, insert or update code blocks, convert structured content into DingTalk document content, or provide repeatable DingTalk document editing procedures.
---

# 钉钉文档

## 核心流程

1. 确认目标文档。
   - 如果用户已经打开 `alidocs.dingtalk.com` 页面，默认使用当前页面作为目标文档，除非用户提供了另一个链接。
   - 如果用户明确要求修改在线文档，将该请求视为对该文档编辑的授权。如果用户只是要求补充 skill 或询问做法，不要修改在线文档。
   - 如果登录、权限或 CAPTCHA 阻塞页面，让用户先在浏览器中处理。

2. 对在线钉钉文档使用浏览器控制。
   - 钉钉文档正文通常渲染在 `iframe#wiki-doc-iframe` 中。
   - 读取正文和执行编辑动作时，优先把操作范围限定在这个 iframe 内。
   - 外层页面 DOM 可能只暴露导航壳，不要用外层 `document.body.innerText` 判断正文。

3. 选择编辑方式。
   - 长篇生成内容：先生成干净的 Markdown/纯文本，再粘贴到编辑器，最后检查渲染结果。
   - 表格、图片、代码块、公式、流程图、任务、日期、内嵌网页等原生块：优先使用钉钉文档的 `插入` 菜单或斜杠命令。
   - 修改已有内容：保留周边内容，只改用户要求的范围，并检查最终渲染效果。

4. 每次有意义的修改后都要验证。
   - 检查 iframe 文本、可见格式，以及 `上次编辑` 一类自动保存状态。
   - 表格、图形、多栏布局、插入组件等布局敏感内容要用截图辅助确认。

## 参考资料

当任务涉及以下操作时，读取 `references/quick-operations.md`：

- 快速读取钉钉文档内容。
- 快速修改钉钉文档内容。
- 在指定位置插入表格。
- 修改表格内容。
- 在指定位置插入流程图或时序图。
- 修改流程图或时序图内容。
- 在指定位置插入代码块。
- 修改代码块内容。

当任务需要插入菜单项名称、iframe 结构或编辑器界面细节时，读取 `references/editor-ui.md`。

## 实用规则

- 斜杠命令优先使用中文完整命令名，例如 `/表格`，不要优先使用可能冲突的短别名。
- 不要依赖外层页面的 `document.body.innerText` 读取正文；要检查编辑器 iframe。
- 除非用户明确要求整体重写，否则不要整篇替换已有文档。
- 插入生成内容前，先确定插入位置：标题、当前光标、文档末尾、某个标题前后，或某个表格单元格内。
- UI 菜单打开后，读取可见菜单文本并选择精确项名。钉钉文档的 CSS class 可能频繁变化。
