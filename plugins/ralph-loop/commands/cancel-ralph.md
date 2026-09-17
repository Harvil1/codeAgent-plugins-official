---
description: "Cancel active Ralph Loop"
allowed-tools: ["terminal(test -f .claude/ralph-loop.local.md:*)", "terminal(rm .claude/ralph-loop.local.md)", "read_file(.claude/ralph-loop.local.md)"]
hide-from-slash-command-tool: "true"
---

# Cancel Ralph

To cancel the Ralph loop:

1. Check if `.claude/ralph-loop.local.md` exists using terminal: `test -f .claude/ralph-loop.local.md && echo "EXISTS" || echo "NOT_FOUND"`

2. **If NOT_FOUND**: Say "No active Ralph loop found."

3. **If EXISTS**:
   - Read `.claude/ralph-loop.local.md` to get the current iteration number from the `iteration:` field
   - Remove the file using terminal: `rm .claude/ralph-loop.local.md`
   - Report: "Cancelled Ralph loop (was at iteration N)" where N is the iteration value
