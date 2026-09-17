---
name: example-command
description: An example user-invoked load_skill that demonstrates frontmatter options and the skills/<name>/SKILL.md layout
---

# Example Command (Skill Format)

This demonstrates the `skills/<name>/SKILL.md` layout for user-invoked slash commands. It is functionally identical to the legacy `commands/example-command.md` format — both are loaded the same way; only the file layout differs.

## Arguments

The user invoked this with: 用户在命令后敲的参数（见本消息末尾原文）

## Instructions

When this skill is invoked:

1. Parse the arguments provided by the user
2. Perform the requested action with the tools you need (`read_file`, `glob`, `search_files`, `terminal`, ...)
3. Report results back to the user

## Frontmatter Options Reference

Skills in CodeAgent support these frontmatter fields:

- **name**: Skill identifier (matches directory name)
- **description**: Short description shown in /help
- **user-invocable**: `false` hides it from the user's slash-command list (model can still `load_skill` it)
- **context**: `fork` makes it run in an isolated sub-agent
- **files**: Extra reference files to attach when the skill loads
- **allowed-tools / disallowed-tools**: Tool names as an exact-match list (e.g. `read_file`, `terminal`) — narrows the visible toolset for the rest of the session once this skill loads, so use sparingly

<!-- CodeAgent 适配注：原 Claude Code 的 argument-hint / allowed-tools: [Read, Glob, Grep, Bash] /
     model 三个键本项目不认，已移除——CC 工具名（Read/Bash 等）在本项目按整名匹配会滤空工具表。 -->

## Example Usage

```
/example-command my-argument
/example-command arg1 arg2
```
