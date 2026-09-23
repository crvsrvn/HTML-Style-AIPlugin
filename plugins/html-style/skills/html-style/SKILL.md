---
name: html-style
description: 创建、修改、重排或发布任何 HTML 成品时必须使用——包括 HTML 文档、报告、方案、说明页、仪表盘、Claude Artifacts、把 Markdown 转成 HTML、更新已有 Artifact 链接。统一样式：Comic Sans MS 16px、右侧大纲、底色 #FCFCFB / #151515、988px 居中版心、SVG 等图表可左右键拖动、滚轮定点缩放、双击全屏，并尽量用原生 HTML/CSS 提升可读性；成品托管到 AI 云端（如 Claude Artifacts），不交付本地 .html 文件。
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
| 背景 | 页面底色浅色 `#FCFCFB`，深色 `#151515`，跟随系统 / 宿主主题 |
| 大纲 | 放在右侧：固定的整高侧栏，由模板脚本从标题生成，可筛选，随滚动高亮；视口窄于 900px 时收进右上角「大纲」按钮，从右侧滑出 |
| 版心 | 固定宽度 988px，在大纲左侧的区域内水平居中；空间不足时收窄，两侧至少留 16px |
| 图表 | SVG、图片化图表、canvas、Mermaid 一律放进 `.hs-zoom` 视窗：<br>• 左键、右键都能拖动平移<br>• 滚轮在任意位置以指针为中心缩放<br>• 双击进入全屏，全屏里同样可拖动、缩放<br>• 再次双击或按 Esc 退出全屏<br>• 「复位」按钮回到适应尺寸 |
| 可读性 | 尽量使用原生 HTML / CSS 能力让文档易读（见[原生可读性增强](#原生可读性增强)）；能用原生能力实现的交互，不写脚本 |
| 交付 | 托管到 AI 云端并给出链接，不把本地 `.html` 当成品（见[托管](#托管)） |

其余视觉参数（配色、字号层级、表格与代码块样式）以模板为准，不要另起一套。

## 原生可读性增强

以下能力都不依赖脚本，按内容需要自由组合。

| 能力 | 写法 | 用在哪里 |
|------|------|----------|
| 折叠代码 | `<details class="hs-code" [open]><summary>语言<span class="hs-code-meta">N 行</span></summary><pre><code>…</code></pre></details>` | 带语言标注的代码块；超过 25 行默认折叠，其余默认展开 |
| 折叠段落 | `<details><summary>标题</summary>…</details>` | 附录、推导过程、次要细节、长清单 |
| 术语表 | `<dl><dt>术语</dt><dd>解释</dd></dl>` | 术语、字段、参数说明 |
| 脚注弹层 | `<button type="button" class="hs-ref" popovertarget="fn-1">1</button>` + `<div popover id="fn-1" class="hs-pop">…</div>` | 脚注、补充解释，不打断正文 |
| 提示块 | `<aside>…</aside>`；警告用 `<aside data-kind="warn">` | 注意事项、结论摘要 |
| 行内语义 | `<kbd>`、`<mark>`、`<abbr title="…">` | 按键、重点句、缩写 |
| 表格 | `<div class="hs-table"><table><caption>…</caption>…</table></div>` | 模板自带表头底色、斑马纹、悬停高亮、窄屏横向滚动 |
| 标题编号与锚点 | `<span class="hs-num">2.1</span>`、`<a class="hs-anchor" href="#id">#</a>` | 长文档的章节编号与可分享链接 |
| 阅读进度与回到顶部 | 模板里的 `.hs-progress` 与 `.hs-top` 两行标记 | 所有长文档，纯 CSS 滚动驱动 |
| 跳转高亮 | 模板自带 `:target` 样式 | 从大纲或链接跳转后短暂高亮目标 |

原则：

- 折叠只用于次要内容；结论、关键数据不折叠。
- 不要把正文拆成只能靠脚本显示的片段。`<details>` 的内容在浏览器页内搜索中仍可命中。

## 工作流程

### 新建页面

1. 读取本技能目录下的 `assets/template.html`。
2. 以下几块原样复制，不要改写、删减或"重新实现"：
   - 字体 `<link>`
   - `<style id="hs-style">`
   - `<div class="hs-progress">` 与 `<a class="hs-top">` 两行标记
   - `<script id="hs-script">`
3. 正文全部写在 `<main class="hs-doc">` 里：
   - 一个 `h1` 作标题，`id="top"`，其后可跟 `<p class="hs-meta">` 写日期、来源。
   - 章节用 `h2` / `h3`，默认这两级进大纲；要收录 `h4` 时，给 `main` 加 `data-toc-levels="2,3,4"`。
   - 不要手写大纲，脚本会生成。
4. 图表写成 `<figure><div class="hs-zoom">img / svg / canvas</div><figcaption>…</figcaption></figure>`：
   - `figure` 的直接子级 img / svg / canvas，以及 Mermaid 渲染出的 SVG，会被脚本自动包进 `.hs-zoom`。
   - 底板：图片默认白色底板，适合 PlantUML、Mermaid 导出图、截图这类自带白底和深色线条的图。
   - 内联 SVG 和 canvas 默认用面板色底板，要求用 `var(--hs-*)` 跟随主题配色。
   - 需要反过来时，在 `.hs-zoom` 上设 `data-plate="light"` 或 `data-plate="none"`。
5. 按[原生可读性增强](#原生可读性增强)组织内容。
6. 页面特有样式另起一个 `<style>`：只用 `--hs-*` 令牌取色，不覆盖硬性项。
7. 宿主要求完整文档时（非 Artifact），外面包一层 `<!doctype html><html lang="zh-CN"><head>…</head><body>…</body></html>`：
   - `head` 里放 `<meta charset="utf-8">`、`<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">`，以及 `title`、字体 `link`、`style`。
   - 其余标记和 `script` 放在 `body` 里，`script` 放在末尾。

### 修改已有页面

1. 先取回线上最新版本，基于它修改。Claude Artifact 用 Artifact 工具 `action: "read"` 读取。
2. 保留全部内容：文字、标题 `id`、页内锚点、表格、代码高亮、图片数据、附带文件（如 PlantUML 源文件）。
3. 删除以下旧结构：
   - 旧字体链接和全局样式。
   - 旧大纲，包括目录、筛选框和目录脚本。
   - 旧的图片缩放、切换按钮及其脚本。
4. 按"新建页面"第 2–6 步套上模板：
   - 旧类名换成模板类名，例如标题编号 → `hs-num`，锚点 → `hs-anchor`，表格容器 → `hs-table`，带语言标注的代码块 → `details.hs-code`。
   - 模板没有对应物的旧类名，在页面特有样式里改用 `--hs-*` 令牌。
5. 页面较大（例如含大量 base64 图）时，用脚本做结构替换，不要手工逐段改。替换前后核对标题、表格、代码块、图片的数量一致。

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
- [ ] 页面底色为 `#FCFCFB` / `#151515`，浅色和深色主题下文字都清晰。
- [ ] 视口 ≥900px 时大纲是右侧整高侧栏；更窄时有「大纲」按钮和右侧抽屉。
- [ ] 版心最宽 988px 并居中，页面没有横向滚动；只有表格、代码块、图表在自己的容器内滚动或缩放。
- [ ] 每个图表都能左键、右键拖动，滚轮以指针为中心缩放，双击全屏，全屏内同样可拖动缩放，双击或 Esc 退出。
- [ ] 长代码、附录等次要内容已用原生折叠；没有为原生能力可以实现的交互写脚本。
- [ ] 交付的是云端链接，或已如实说明无法托管。
