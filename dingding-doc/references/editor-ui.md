# DingTalk Editor UI Reference

Use this reference for browser-based editing of DingTalk documents at `alidocs.dingtalk.com`.

## Editor Surface

- The main document editor is usually inside `iframe#wiki-doc-iframe`.
- The iframe toolbar can expose: `菜单`, `插入`, `AI 创作`, paragraph style controls such as `默认` and `正文`, plus `添加图标`, `添加封面`, and `设置文档信息`.
- The blank editor placeholder can show `输入“/”插入或`.
- Use iframe-scoped selectors and text checks when automating.

Example read:

```js
const frame = tab.playwright.frameLocator('#wiki-doc-iframe');
const text = await frame.locator('body').innerText({ timeoutMs: 5000 });
```

## Insert Menu

Open toolbar `插入` to access native document blocks. Observed groups and items:

- `基础 / 通用`: `图片`, `表格`, `文件`, `同步块`, `代码块`, `在线文档`, `模板内容`, `表情`, `标签`, `公式`
- `样式布局`: `高亮块`, `分栏`, `分割线`, `素材库`
- `数据`: `电子表格`
- `画板图形`: `白板`, `脑图`, `路线图`, `流程图`, `文本绘图`
- `团队协作`: `提及人`, `群聊`, `Agoal`, `钉钉日程`, `视频会议`, `AI 听记`, `知识库`, `信息收集`, `投票`
- `项目管理`: `项目任务`, `项目看板`
- `小工具`: `目录`, `表态`, `日期`, `投屏`, `计时器`, `倒计时`, `手写签名`, `模板按钮`, `内嵌网页`, `三方应用`, `企业内应用`

Some visible shortcut hints can collide, for example `图片`, `投票`, and `投屏` may all show `/tp`. Prefer full Chinese keywords in slash commands.

## Add A Table

For the full table insertion and modification workflow, read `quick-operations.md`.

Use one of these paths:

1. Toolbar path:
   - Click the target insertion point in the document body.
   - Click `插入`.
   - Choose `表格` under `基础 / 通用`.
   - If a size picker appears, select the required rows and columns.

2. Slash-command path:
   - Click the target insertion point in the document body.
   - Type `/表格`.
   - Choose the visible command named `表格`.
   - Confirm the table size if prompted.

To fill a table from data:

```text
列一	列二	列三
值 1	值 2	值 3
值 4	值 5	值 6
```

- Prepare tab-separated values for rows and columns.
- Insert the table first when an exact grid is required.
- Click the top-left target cell and paste the TSV block.
- Verify that each value landed in the intended cell. If DingTalk pastes into one cell, fill cells manually with `Tab` navigation.

## Add Long Structured Content

- Draft content as Markdown when possible: headings, bullet lists, numbered lists, task lists, block quotes, and fenced code blocks.
- Paste into the editor body for bulk content creation.
- Use native insert blocks after the paste for components that Markdown cannot reliably represent, such as DingTalk tables, flowcharts, project tasks, dates, mentions, and embedded webpages.
- After pasting, check that heading levels, lists, and code blocks rendered correctly.

## Modify Existing Content

For the full quick-read, quick-modify, table, diagram, and code-block workflows, read `quick-operations.md`.

1. Read the current iframe text or inspect the visible area.
2. Locate the specific heading, paragraph, table, or block to change.
3. Make the smallest edit that satisfies the request.
4. Verify the changed text and visible layout.

For risky edits, first produce a short change plan and ask for confirmation if the user did not clearly authorize the document mutation.
