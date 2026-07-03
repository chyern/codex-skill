---
name: dingding-doc
description: Operate DingTalk/Alibaba Docs (钉钉文档/阿里文档) in the web editor for agent-driven document work. Use when Codex must read, generate, paste, edit, format, or verify an alidocs.dingtalk.com document; convert structured content into DingTalk document content; insert or update tables, flowcharts, sequence diagrams, code blocks, formulas, dates, layout blocks, icons, or covers; or provide repeatable browser procedures for modifying DingTalk documents.
---

# 钉钉文档 Agent 手册

## 执行契约

- 由 agent 自己生成、改写和整理内容；不要使用钉钉文档内置 `AI 创作` 或任何 AI 生成入口。
- 只有用户明确要求修改在线文档时才编辑页面。用户只是询问 skill、流程或能力时，不要改在线文档。
- 如果用户已经打开 `alidocs.dingtalk.com` 文档，默认把当前页面作为目标文档；用户提供其他链接时再切换。
- 登录、权限、CAPTCHA 或企业选择阻塞时，让用户在浏览器中处理后再继续。
- 上传本地文件、选择企业文档、发送外部数据、修改分享/权限前，必须有明确授权。

## 基本浏览器模型

- 正文通常在 `iframe#wiki-doc-iframe` 中。读取正文和执行编辑动作时，优先限定在这个 iframe。
- 不要用外层 `document.body.innerText` 判断正文；外层页面常只暴露导航壳。
- 用 `innerText` 读取渲染文本；`textContent` 容易混入脚本或隐藏状态。
- 工具栏文字可见但不一定暴露稳定 `button`/role。点击菜单后必须读取展开项或截图确认；未展开时停止盲目点击，改用中文斜杠命令或请用户手动打开菜单。

## 工作流

1. 判定任务类型。
   - 读取/总结文档：读取 iframe 文本，必要时截图确认布局。
   - 生成文档内容：先生成 Markdown/纯文本草稿，再粘贴到目标位置；表格、图形、代码等用原生块补齐。
   - 修改已有内容：定位目标段落、标题、表格、图形或代码块，只改用户要求的范围。
   - 格式化：使用 `正文` 样式菜单设置正文或 `标题1` 到 `标题6`；字体使用 `默认` 字体菜单。
   - 插入原生块：优先中文斜杠命令，例如 `/表格`、`/代码块`、`/流程图`、`/文本绘图`；菜单路径必须基于当前可见项。

2. 选择并读取 reference。
   - 读写、生成、修改、表格、图形、代码块：读取 `references/quick-operations.md`。
   - 需要 UI 入口、斜杠命令、插入菜单项名：读取 `references/editor-ui.md`。
   - 需要已验证能力、标题/正文样式、字体、图标、封面、菜单分类：读取 `references/verified-editor-capabilities.md`。

3. 执行前确认插入点或修改范围。
   - 可接受位置：当前光标、文档末尾、某标题前后、某段文字前后、某个表格单元格、代码块或图形块内部。
   - 用户没有指定位置时，默认文档末尾或当前光标，但先说明这个假设。
   - 除非用户明确要求整体重写，否则不要整篇替换已有文档。

4. 生成可粘贴载荷。
   - 长篇内容：Markdown/纯文本，保持标题、列表、引用和 fenced code block。
   - 表格：先准备 TSV；需要原生表格时先插表再粘 TSV。
   - 流程图/时序图：优先原生 `流程图` 或 `文本绘图`；不可用时插入 Mermaid 源码代码块并说明是源码 fallback。
   - 代码：使用代码块，粘贴完整代码，避免逐字符输入。

5. 修改后必须验收。
   - 读取 iframe 文本确认内容。
   - 用截图确认标题层级、表格边界、代码块、图形、分栏、高亮块等布局敏感内容。
   - 检查自动保存状态，例如 `已保存` 或 `上次编辑`。
   - 如果验证结果与预期不一致，修正后再次验收；不能确定时说明不确定点。

## 关键规则

- 不要把未在当前页面验证过的菜单项写成确定能力或直接执行。
- 菜单项打开后，选择精确可见文本；不要依赖 CSS class 或猜测坐标。
- 中文斜杠命令优先用完整名称，避免 `/tp` 这类短别名冲突。
- 保留周边内容和文档结构；批量修改前先列出目标项。
- 对高风险操作先给简短计划并确认：整篇替换、删除大块内容、替换整张表、上传文件、改封面/图标、修改权限。
