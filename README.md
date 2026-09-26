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
| 图表 | SVG、图片、canvas、Mermaid：左键 / 右键拖动平移，滚轮以指针为中心缩放，双击网页全屏（铺满当前窗口，不调用浏览器全屏）；全屏内交互相同，双击或 Esc 退出 |
| 表格 | 右上角网页全屏按钮；全屏内横纵滚动、表头吸顶，Esc 退出 |
| 可读性 | 优先用原生 HTML / CSS：折叠代码与附录、术语表、脚注弹层、提示块、回到顶部、跳转高亮 |
| 交付 | Claude：Artifact；Codex：`sites-hosting` 等托管技能，不可用时如实说明 |

完整规则见 [SKILL.md](plugins/html-style/skills/html-style/SKILL.md)，可直接运行的样式、脚本与示例见 [template.html](plugins/html-style/skills/html-style/assets/template.html)。

## 安装

Claude Code 与 Codex 用插件市场安装；其他工具见[仅安装 Skill](#仅安装-skill)。下文 `<市场来源>` 二选一：

- GitHub：`crvsrvn/HTML-Style-AIPlugin`，或 fork 后的 `<owner>/<repo>`；也可以传 HTTPS / SSH 克隆地址。
- 本地目录：仓库克隆或解压后的根目录，即包含 `.claude-plugin/marketplace.json` 的目录，放在哪里都可以。写绝对路径；Claude Code 也接受以 `./`、`../` 开头的相对路径，不带前缀会被当成 GitHub 的 `owner/repo`。

固定到某个分支或标签时：Claude Code 在来源后加 `#<ref>`；Codex 用 `owner/repo@<ref>`，或加 `--ref <ref>`。

### 让 AI 安装（推荐）

在 Claude Code 或 Codex 里直接说：

```text
按 README 安装这个插件：https://github.com/crvsrvn/HTML-Style-AIPlugin
```

也可以把链接换成本地克隆的路径。

> **给执行安装的 AI**
>
> 1. 判断当前平台是 Claude Code 还是 Codex，只执行对应平台的命令；都不是时按「仅安装 Skill」一节处理。
> 2. 确定 `<市场来源>`：用户给的是 GitHub 地址就直接用；给的是本地路径就先解析成绝对路径，并确认其中存在 `.claude-plugin/marketplace.json`（Codex 为 `.agents/plugins/marketplace.json`）。不要沿用任何示例路径。
> 3. `claude` / `codex` 不在 PATH 时，先定位本机的 CLI 可执行文件再调用（例如 Claude 桌面版自带的 CLI），不要假设固定路径。
> 4. 市场已用同一来源添加过时，跳过添加这一步；来源不同（例如从本地路径换成 GitHub）时，先移除旧市场（`claude` 或 `codex` 加 `plugin marketplace remove html-style-aiplugin`）再添加。已安装过则按「更新」一节操作。
> 5. Codex CLI 没有 `codex plugin add` 子命令时（较旧版本），添加市场后请用户在 Codex 里用 `/plugins` 安装。
> 6. 完成后告诉用户：开始新的对话 / 任务后生效。

### Claude Code

```bash
claude plugin marketplace add <市场来源>
claude plugin install html-style@html-style-aiplugin
```

默认装到用户级，所有项目都可用。桌面版 Code 标签页与 CLI 共用同一份用户设置：用上面的命令添加市场后，也可以在输入框旁的 **+** → **Plugins** → **Add plugin** 里安装。安装后开始新的对话即可生效。

### Codex

```bash
codex plugin marketplace add <市场来源>
codex plugin add html-style@html-style-aiplugin
```

较旧的 Codex CLI 没有 `codex plugin add`，添加市场后在 Codex 里用 `/plugins` 安装。安装后开始新的 Codex 任务即可生效。

### 仅安装 Skill

插件只含一个 Skill 目录（`SKILL.md` + `assets/template.html`），不依赖安装位置，可以不经插件系统直接使用，适合其他支持 Agent Skills（`SKILL.md`）的工具：

- 本地工具：把 `plugins/html-style/skills/html-style/` 整个目录复制到该工具的用户级 Skills 目录，目录名保持 `html-style`，例如 Claude Code 的 `~/.claude/skills/html-style/`；其他工具的目录以其文档为准。
- claude.ai：把该目录打包成 ZIP，在 **Customize** → **Skills** 里上传（需开启代码执行）。

与插件安装二选一，避免同一个 Skill 加载两份。更新时重新复制或上传。

### 更新

Claude Code：

```bash
claude plugin marketplace update html-style-aiplugin
claude plugin update html-style@html-style-aiplugin
```

`claude plugin update` 按插件清单里的版本号判断有没有新版本，版本号没变时不会更新。

Codex 安装的是快照，先刷新市场（仅 Git 来源需要第一条），再重装：

```bash
codex plugin marketplace upgrade html-style-aiplugin
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
    assets/template.html                   样式、图标、大纲脚本、图表缩放脚本、图表与表格全屏脚本、示例
```

## 使用

不需要记命令。直接说"把这份方案做成 HTML 文档""按样式规范改一下这个 Artifact：<链接>"即可，Skill 会在涉及 HTML 的任务里自动触发。

## 维护

- 改样式或脚本时只改 `template.html`，同步更新 `SKILL.md` 里的规则。
- 每次发布改动都提升版本号，否则 Claude Code 不会更新已安装的插件；各清单文件与 `template.html` 里的版本号保持一致。

## 许可证

MIT
