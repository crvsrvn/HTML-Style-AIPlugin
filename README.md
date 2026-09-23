# HTML Style

同时支持 Claude Code 与 Codex 的 HTML 样式规范插件。AI 每次创建或修改 HTML 时，都会套用同一套样式并把成品托管到云端，不交付本地 `.html` 文件。

插件只包含一个 Skill 和一份模板，没有 MCP 服务，也不需要 Node / Python 运行时。

## 规范一览

| 项 | 规范 |
|----|------|
| 字体 | Comic Sans MS，16px，行高 1.75；回退到 Comic Neue、系统中文字体 |
| 大纲 | 右侧，从标题自动生成、随滚动高亮；窄于 1240px 时收成右上角按钮 + 右侧抽屉 |
| 背景 | Claude 桌面版页面底色：浅色 `#FCFCFB`，深色 `#151515`，跟随主题 |
| 版心 | 居中，固定 760px |
| 图表 | SVG、图片、canvas、Mermaid：左键 / 右键拖动平移，滚轮以指针为中心缩放，双击复位 |
| 交付 | Claude：Artifact；Codex：`sites-hosting` 等托管技能，不可用时如实说明 |

完整参数（标题比例、段距、代码、表格、颜色）见 [SKILL.md](plugins/html-style/skills/html-style/SKILL.md)，可直接运行的样式与脚本见 [template.html](plugins/html-style/skills/html-style/assets/template.html)。

## 安装

### Claude Code

```bash
claude plugin marketplace add E:\Repositories\HTML-Style-AIPlugin
claude plugin install html-style@html-style-aiplugin
```

桌面版对应 `/plugin marketplace add`、`/plugin install` 两条斜杠命令。安装后开始新的对话即可生效。

### Codex

```powershell
codex plugin marketplace add "E:\Repositories\HTML-Style-AIPlugin"
codex plugin add html-style@html-style-aiplugin
```

安装后开始新的 Codex 任务即可生效。

### 通过 Git 分享

仓库推到 Git 服务后，接收方直接添加仓库市场：

```bash
# Claude Code
claude plugin marketplace add OWNER/HTML-Style-AIPlugin
claude plugin install html-style@html-style-aiplugin

# Codex
codex plugin marketplace add OWNER/HTML-Style-AIPlugin --ref main
codex plugin add html-style@html-style-aiplugin
```

把 `OWNER` 换成实际的 GitHub 组织或用户名，也可以传 HTTPS / SSH 地址。

## 目录结构

```
.claude-plugin/marketplace.json            Claude Code 市场清单
.agents/plugins/marketplace.json           Codex 市场清单
plugins/html-style/
  .claude-plugin/plugin.json               Claude Code 插件清单
  .codex-plugin/plugin.json                Codex 插件清单
  skills/html-style/
    SKILL.md                               规范与工作流程（两个平台共用）
    assets/template.html                   样式、大纲脚本、图表缩放脚本与示例
```

## 使用

不需要记命令。直接说"把这份方案做成 HTML 文档""按样式规范改一下这个 Artifact：<链接>"即可，Skill 会在涉及 HTML 的任务里自动触发。

## 设计依据

- **颜色**：取自 Claude 桌面版 v2.7032 的 cds 设计令牌。
  - 页面底色：`index.html` 声明 `--cds-page-bg: var(--cds-surface-1)`，`theme-color` 为 `#fcfcfb` / `#151515`。
  - 文字：`--cds-text-primary` / `--cds-text-secondary`。
  - 强调色：`--cds-text-accent`。
- **排版**：
  - 版心宽度参考 Obsidian 700px 与 Typora 860px 取中。
  - 行高在 Typora 1.6 的基础上为中文放宽到 1.75。
  - 标题紧、正文松的层级参考 Apple 官网。

## 维护

- 改样式或脚本时只改 `template.html`，同步更新 `SKILL.md` 里的参数表。
- 四个清单文件里的版本号保持一致。

## 许可证

MIT
