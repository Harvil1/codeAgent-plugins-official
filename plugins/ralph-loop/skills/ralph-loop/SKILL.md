---
name: ralph-loop
description: Start Ralph Loop in current session
---

<!-- CodeAgent 适配注：原版靠 Claude Code 的 Stop-hook 在退出时回灌同一提示词循环；本环境没有插件级 Stop-hook，循环续跑请配合 /goal（goal_start）把同一提示词设为迭代目标，脚本只负责初始化标记文件。 -->
# Ralph Loop Command

Execute the setup script to initialize the Ralph loop:

用 terminal 工具运行（插件已装在本机时路径如下，参数接在后面）：

```
~/.codeAgent/plugins/ralph-loop/scripts/setup-ralph-loop.sh <参数>
```

Please work on the task. When you try to exit, the Ralph loop will feed the SAME PROMPT back to you for the next iteration. You'll see your previous work in files and git history, allowing you to iterate and improve.

CRITICAL RULE: If a completion promise is set, you may ONLY output it when the statement is completely and unequivocally TRUE. Do not output false promises to escape the loop, even if you think you're stuck or should exit for other reasons. The loop is designed to continue until genuine completion.
