# CodeAgent Plugins Official

English | [简体中文](./README.md)

The official plugin marketplace for [CodeAgent](https://github.com/Harvil1/codeAgent), a self-learning AI agent CLI: a directory of skills, sub-agents, and MCP extensions.

> **This marketplace is adapted from Anthropic's [claude-plugins-official](https://github.com/anthropics/claude-plugins-official)** (under its Apache-2.0 license) — many thanks to the original authors and all plugin authors. The bundled plugins have been systematically adapted for CodeAgent: tool names (`Read`→`read_file`, `Bash`→`terminal`), path conventions (`CLAUDE.md`→`CODEAGENT.md`, `CLAUDE_PLUGIN_ROOT`→CodeAgent plugin paths), frontmatter, and doc references were all rewritten — see the "CodeAgent 适配注" (adaptation notes) inside each plugin.

> **⚠️ Important:** Make sure you trust a plugin before installing, updating, or using it. The marketplace maintainers cannot control what MCP servers, files, or other software are included in plugins, and cannot verify that they will work as intended. See each plugin's homepage for more information.

## Structure

- **`/plugins`** — Official plugins maintained by this project
- **`/external_plugins`** — Third-party plugins from partners and the community

## Installation

Inside a CodeAgent session:

```
/plugins                                           # Browse the marketplace, search, Enter to install
/plugin install <plugin-name>@claude-plugins-official    # Install by name
```

Once installed, a plugin's skills are immediately available (invoke as `/<skill-name>` or auto-loaded by the model via `load_skill`); its `agents/` sub-agent definitions and `.mcp.json` MCP servers are wired up as well.

## Adaptation Notes

What gets rewritten when porting a plugin from the Claude ecosystem (summary):

| Original (Claude Code) | Adapted (CodeAgent) |
|---|---|
| Tool names like `Read` / `Bash` / `Grep` / `TodoWrite` | `read_file` / `terminal` / `search_files` / `task_create` |
| `CLAUDE.md` / `CLAUDE_PLUGIN_ROOT` | `CODEAGENT.md` / `~/.codeAgent/plugins/<plugin-name>` |
| `commands/*.md` command files | `skills/<name>/SKILL.md` skills (same slash-command experience) |
| Frontmatter `allowed-tools: Read, terminal(git:*)` | Removed (CodeAgent matches exact tool names; CC-style specs would empty the toolset) |
| Inline execution `` !`cmd` ``, `$1` positional args, `@file` references | Rewritten as natural-language instructions (the model runs/reads via its tools) |

## Plugin Structure

```
plugin-name/
├── .claude-plugin/
│   └── plugin.json      # Plugin manifest (required; a root-level plugin.json also works)
├── .mcp.json            # MCP server configuration (optional)
├── agents/              # Sub-agent definitions (optional)
├── skills/              # Skills (optional; one directory per skill with SKILL.md)
└── README.md            # Documentation
```

## Contributing

PRs welcome. If you're porting a plugin from the Claude ecosystem, please adapt it per the table above first (CodeAgent also applies an automatic tool/product-name rewriting pass at install time as a fallback). Make sure your plugin ships a valid `plugin.json` and its skills use `name` + `description` frontmatter.

### Plugin names are immutable

The `name` field of a marketplace entry is an **immutable slug**: once published, don't change it — users have it installed under that slug, and renaming breaks their install with a `plugin-not-found` error. If a rename is truly unavoidable, add an entry to the top-level `renames` map in `.claude-plugin/marketplace.json` so existing installs auto-migrate.

### Skill-bundle plugins

When the source repository ships `SKILL.md` files without a `plugin.json` manifest, the marketplace entry can declare the skills directly using `strict: false` and an explicit `skills` array:

```json
{
  "name": "example-bundle",
  "description": "Brief description of the bundled skills.",
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

Each path in `skills` resolves relative to `source.path` and points at a directory containing a `SKILL.md`.

## License

[Apache-2.0](./LICENSE) — carried over from upstream [claude-plugins-official](https://github.com/anthropics/claude-plugins-official); each plugin's own license, if any, is stated in its directory.
