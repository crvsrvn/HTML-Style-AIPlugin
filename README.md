# HTML Style

同时支持 Claude Code 与 Codex 的 HTML 样式规范插件。AI 每次创建或修改 HTML 时，都会套用同一套样式并把成品托管到云端，不交付本地 `.html` 文件。

插件只包含一个 Skill 和一份模板，没有 MCP 服务，也不需要 Node / Python 运行时。

## 规范一览

| 项 | 规范 |
|----|------|
| 字体 | Comic Sans MS，16px；回退到 Comic Neue、系统中文字体 |
| 背景 | 浅色 `#FCFCFB`，深色 `#151515`，跟随主题 |
| 大纲 | 右侧整高侧栏，无底色，从标题自动生成，可筛选、随滚动高亮；默认折叠，随点击或滚动到的位置逐级展开当前路径、收起其余分支，图标按钮切换展开全部；窄于 900px 时收成右上角按钮 + 右侧抽屉 |
| 页内跳转 | 所有跳转都直接跳转：大纲、正文、表格里的页内链接、回到顶部、浏览器前进 / 后退 |
| 标题与强调 | 标题带序号（与标题同色）、不加图标；加粗与高亮（`<mark>`）二选一 |
| 色彩与标记 | 语义提示块（说明 / 建议 / 注意 / 风险 / 要点 / 备注）；状态标签、图例记号、ID 自动着色；内置 33 个线性图标 |
| 版心 | 固定 988px，在大纲左侧区域内居中 |
| 图表 | SVG、图片、canvas、Mermaid：左键 / 右键拖动平移，滚轮以指针为中心缩放，双击全屏；全屏内交互相同，双击或 Esc 退出 |
| 可读性 | 优先用原生 HTML / CSS：折叠代码与附录、术语表、脚注弹层、提示块、回到顶部、跳转高亮 |
| 交付 | Claude：Artifact；Codex：`sites-hosting` 等托管技能，不可用时如实说明 |

完整规则见 [SKILL.md](plugins/html-style/skills/html-style/SKILL.md)，可直接运行的样式、脚本与示例见 [template.html](plugins/html-style/skills/html-style/assets/template.html)。

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

### 更新

两个平台安装的都是插件快照。改动仓库后需要重装才能生效：

```bash
claude plugin uninstall html-style@html-style-aiplugin
claude plugin install html-style@html-style-aiplugin
```

```powershell
codex plugin remove html-style@html-style-aiplugin
codex plugin add html-style@html-style-aiplugin
```

## 目录结构

```
.claude-plugin/marketplace.json            Claude Code 市场清单
.agents/plugins/marketplace.json           Codex 市场清单
plugins/html-style/
  .claude-plugin/plugin.json               Claude Code 插件清单
  .codex-plugin/plugin.json                Codex 插件清单
  skills/html-style/
    SKILL.md                               规范与工作流程（两个平台共用）
    assets/template.html                   样式、图标、大纲脚本、图表缩放与全屏脚本、示例
```

## 使用

不需要记命令。直接说"把这份方案做成 HTML 文档""按样式规范改一下这个 Artifact：<链接>"即可，Skill 会在涉及 HTML 的任务里自动触发。

## 维护

- 改样式或脚本时只改 `template.html`，同步更新 `SKILL.md` 里的规则。
- 四个清单文件里的版本号保持一致。

## 许可证

MIT
