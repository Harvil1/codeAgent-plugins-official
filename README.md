# CodeAgent 官方插件市场

[English](./README.en.md) | 简体中文

[CodeAgent](https://github.com/Harvil1/codeAgent)（自学习 AI Agent 命令行工具）的官方插件市场：技能 / 子代理 / MCP 扩展目录。

> **本市场基于 Anthropic 的 [claude-plugins-official](https://github.com/anthropics/claude-plugins-official) 修改适配而来**（遵循其 Apache-2.0 许可证），感谢原作者与所有插件作者。收录插件已针对 CodeAgent 体系系统适配：工具名（`Read`→`read_file`、`Bash`→`terminal`）、路径体系（`CLAUDE.md`→`CODEAGENT.md`、`CLAUDE_PLUGIN_ROOT`→本项目插件路径）、frontmatter 与文档引用均已改写，详见各插件内的「CodeAgent 适配注」。

> **⚠️ 安全提示**：安装、更新或使用任何插件前，请先确认你信任它。市场维护者无法控制插件里包含哪些 MCP server、文件或软件，也无法保证它们按预期工作。请查看各插件的 homepage 了解详情。

## 结构

- **`/plugins`** —— 官方维护的内置插件
- **`/external_plugins`** —— 来自合作伙伴与社区的第三方插件

## 安装

在 CodeAgent 会话里：

```
/plugins                                           # 逛市场：浏览、搜索、回车安装
/plugin install <插件名>@claude-plugins-official    # 按名直装
```

装好后插件自带的技能立即可用（`/<技能名>` 调用，或由模型自动 `load_skill`），`agents/` 子代理与 `.mcp.json` 的 MCP server 也会一并接线。

## 适配说明

从 Claude 生态移植插件时的改写对照（简表）：

| 原写法（Claude Code） | 适配后（CodeAgent） |
|---|---|
| `Read` / `Bash` / `Grep` / `TodoWrite` 等工具名 | `read_file` / `terminal` / `search_files` / `task_create` |
| `CLAUDE.md` / `CLAUDE_PLUGIN_ROOT` | `CODEAGENT.md` / `~/.codeAgent/plugins/<插件名>` |
| `commands/*.md` 命令文件 | `skills/<名>/SKILL.md` 技能（slash 命令同款体验） |
| frontmatter `allowed-tools: Read, terminal(git:*)` | 移除（本项目按整名精确匹配，CC 写法会把工具表滤空） |
| 行内执行 `` !`cmd` ``、`$1` 位置参数、`@文件` 引用 | 改写为自然语言指令（模型用工具自行执行 / 读取） |

## 插件目录结构

```
plugin-name/
├── .claude-plugin/
│   └── plugin.json      # 插件清单（必需；放在根上的 plugin.json 也可以）
├── .mcp.json            # MCP server 配置（可选）
├── agents/              # 子代理定义（可选）
├── skills/              # 技能（可选，每个技能一个目录 + SKILL.md）
└── README.md            # 文档
```

## 贡献

欢迎提 PR 收录新插件。从 Claude 生态搬来的插件请先按上表适配（安装时本项目也会自动做一轮工具名 / 产品名改写兜底）。提交前请确保插件带有效 `plugin.json`、技能 frontmatter 用 `name` + `description`。

### 插件名不可变

市场条目的 `name` 是**不可变的 slug**：一旦发布就不要改——用户装在這個名字下，改名会让他们的安装报 `plugin-not-found`。确需改名时，在 `.claude-plugin/marketplace.json` 顶层的 `renames` 里加一条 `"旧名": "新名"`，老安装会自动迁移。

### 纯技能束（skill bundle）

源仓库只有 `SKILL.md` 没有 `plugin.json` 清单时，市场条目可以用 `strict: false` + 显式 `skills` 数组直接声明技能目录：

```json
{
  "name": "example-bundle",
  "description": "一句话描述这束技能。",
  "source": {
    "source": "git-subdir",
    "url": "https://github.com/example-org/sdk.git",
    "path": "packages/agent-skills",
    "ref": "main",
    "sha": "<commit sha>"
  },
  "strict": false,
  "skills": ["./skill-a", "./skill-b"]
}
```

`skills` 里的路径相对 `source.path` 解析，每个指向一个含 `SKILL.md` 的目录。

## 许可证

[Apache-2.0](./LICENSE)——沿袭上游 [claude-plugins-official](https://github.com/anthropics/claude-plugins-official)；各插件各自的许可以其目录内声明为准。
