---
name: html-style
description: 创建、修改、重排或发布任何 HTML 成品时必须使用——包括 HTML 文档、报告、方案、说明页、仪表盘、Claude Artifacts、把 Markdown 转成 HTML、更新已有 Artifact 链接。统一样式：Comic Sans MS 16px、右侧大纲、Claude 桌面版背景色、内容居中定宽、SVG 等图表可左右键拖动并以指针为中心滚轮缩放；成品托管到 AI 云端（如 Claude Artifacts），不交付本地 .html 文件。
---

# HTML 样式规范

本规范是用户对 HTML 样式的明确约定：

- 用户在当次请求里给出的不同要求，优先于本规范。
- 本规范优先于通用设计建议，例如 artifact-design 等技能里关于字体、配色、版式的默认建议，也优先于更早的零散约定（如"左侧放大纲"）。
- 本规范不覆盖宿主平台的硬性约束，例如 Artifact 的 CSP 白名单、主题三态、安全区。

## 适用范围

- 新建或修改任何 HTML 成品。
- 修改已有页面时，把整页迁移到本规范，不只改动到的那一段。
- 不适用：用户项目里的网页源码（产品前端、组件、模板引擎文件）。它们遵循项目自身的设计系统。

## 硬性规范

| 项 | 要求 |
|----|------|
| 字体 | 正文 `Comic Sans MS`，16px；回退链见模板 `--hs-font`（Comic Neue → Chalkboard SE → 系统中文字体） |
| 大纲 | 放在版心右侧，由模板脚本从标题自动生成并随滚动高亮；视口窄于 1240px 时收进右上角「大纲」按钮，从右侧滑出 |
| 背景 | Claude 桌面版页面底色（`--cds-surface-1`）：浅色 `#FCFCFB`，深色 `#151515`，跟随系统 / 宿主主题 |
| 版心 | 内容水平居中，固定宽度 760px；窄屏两侧各留 16px |
| 图表 | SVG、图片化图表、canvas、Mermaid 一律放进 `.hs-zoom` 视窗。左键、右键都能拖动平移；滚轮在任意位置以指针为中心缩放；双击或「复位」按钮回到适应宽度 |
| 交付 | 托管到 AI 云端并给出链接，不把本地 `.html` 当成品（见[托管](#托管)） |

## 默认参数

以下参数可以在单页内微调，但不能改动硬性项。

| 参数 | 值 | 依据 |
|------|----|------|
| 正文行高 | 1.75 | Typora 1.6、Obsidian 1.5 针对西文；中文需要更松 |
| 段距 | 1.1em | Typora / Obsidian 约 1em |
| 标题 | h1 36px / h2 26px / h3 20px / h4 17px，粗 700，行高 1.25–1.5 | Apple 官网标题紧、正文松的层级 |
| 标题上距 | h2 2.6em，h3 2em | 章节之间留足呼吸 |
| 代码 | 等宽 14px，行高 1.6，底色面板，圆角 12px | |
| 表格 | 15px，只画横线，表头线加深，窄屏在容器内横向滚动 | 简约品牌官网风格 |
| 页边 | 上 88px，下 160px | |
| 大纲 | 宽 ≤220px，距版心 48px，13.5px | |
| 文字色 | 主 `#0B0B0B` / `#F0EFEC`，次 `#52514E` / `#C3C2B7`，弱 `#6D6B67` / `#898781` | Claude 桌面版 cds 灰阶 |
| 强调色 | `#184F95` / `#6DA7EC` | Claude 桌面版 `--cds-text-accent` |
| 分隔线 | `#E1E0D9` / `#2C2C2A` | Claude 桌面版 cds 灰阶 |

## 工作流程

### 新建页面

1. 读取本技能目录下的 `assets/template.html`。
2. 以下三块原样复制，不要改写、删减或"重新实现"：
   - 字体 `<link>`
   - `<style id="hs-style">`
   - `<script id="hs-script">`
3. 正文全部写在 `<main class="hs-doc">` 里：
   - 一个 `h1` 作标题，其后可跟 `<p class="hs-meta">` 写日期、来源。
   - 章节用 `h2` / `h3`，默认这两级进大纲；要收录 `h4` 时，给 `main` 加 `data-toc-levels="2,3,4"`。
   - 不要手写大纲，脚本会生成。
4. 表格包一层 `<div class="hs-table">`。
5. 图表写成 `<figure><div class="hs-zoom">img / svg / canvas</div><figcaption>…</figcaption></figure>`：
   - `figure` 的直接子级 img / svg / canvas，以及 Mermaid 渲染出的 SVG，会被脚本自动包进 `.hs-zoom`。
   - 底板：图片默认白色底板，适合 PlantUML、Mermaid 导出图、截图这类自带白底和深色线条的图。
   - 内联 SVG 和 canvas 默认透明底板，要求用 `var(--hs-*)` 跟随主题配色。
   - 需要反过来时，在 `.hs-zoom` 上设 `data-plate="light"` 或 `data-plate="none"`。
6. 页面特有样式另起一个 `<style>`：只用 `--hs-*` 令牌取色，不覆盖硬性项。
7. 宿主要求完整文档时（非 Artifact），外面包一层 `<!doctype html><html lang="zh-CN"><head>…</head><body>…</body></html>`：
   - `head` 里放 `<meta charset="utf-8">`、`<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">`，以及 `title`、字体 `link`、`style`。
   - `script` 放在 `body` 末尾。

### 修改已有页面

1. 先取回线上最新版本，基于它修改。Claude Artifact 用 Artifact 工具 `action: "read"` 读取。
2. 保留全部内容：文字、标题 `id`、页内锚点、表格、代码高亮、图片数据、附带文件（如 PlantUML 源文件）。
3. 删除以下旧结构：
   - 旧字体链接和全局样式。
   - 旧大纲，包括左侧或顶部目录、目录筛选框、目录脚本。
   - 旧的图片缩放、切换按钮及其脚本。
4. 按"新建页面"第 2–6 步套上模板。旧结构特有的类名（代码块标题、交叉引用标记、标题锚点等）在页面特有样式里改用 `--hs-*` 令牌。
5. 页面较大（例如含大量 base64 图）时，用脚本做结构替换，不要手工逐段改。

## 托管

- 中间文件只放在会话临时目录（Claude Code 的 scratchpad 或系统临时目录），不放进用户项目。
- **Claude Code / Claude 桌面版**：
  - 用 Artifact 工具发布，回复里给链接。
  - 更新已有 Artifact 时传同一个 `url`，或在同一会话里用同一个文件路径，保持链接不变。
  - 遵守该工具的页面契约：不写 doctype / html / head / body；外部样式只从 Google Fonts 加载。
- **claude.ai 网页**：直接产出 HTML Artifact。
- **Codex**：有 `sites-hosting`（配合 `sites-building`）等托管技能时，用它发布并给出链接。没有可用的托管能力时，明确告诉用户"当前环境无法托管"，给出临时目录里的文件路径，不要声称已发布。
- 用户明确要求本地文件时，按用户要求交付。

## 发布前自检

- [ ] 正文字体为 Comic Sans MS（或其回退），16px。
- [ ] 视口 ≥1240px 时大纲在版心右侧；更窄时有「大纲」按钮和右侧抽屉。
- [ ] 背景为 `#FCFCFB` / `#151515`，浅色和深色主题下文字都清晰。
- [ ] 版心 760px 居中，页面没有横向滚动；只有表格、代码块、图表在自己的容器内滚动或缩放。
- [ ] 每个图表都能左键、右键拖动，滚轮以指针为中心缩放，双击复位。
- [ ] 交付的是云端链接，或已如实说明无法托管。
