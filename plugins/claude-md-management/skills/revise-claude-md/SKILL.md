---
name: revise-claude-md
description: Update CODEAGENT.md with learnings from this session
---


Review this session for learnings about working with CodeAgent in this codebase. Update CODEAGENT.md with context that would help future the agent sessions be more effective.

## Step 1: Reflect

What context was missing that would have helped the agent work more effectively?
- Bash commands that were used or discovered
- Code style patterns followed
- Testing approaches that worked
- Environment/configuration quirks
- Warnings or gotchas encountered

## Step 2: Find CODEAGENT.md Files

```bash
find . -name "CODEAGENT.md" -o -name ".claude.local.md" 2>/dev/null | head -20
```

Decide where each addition belongs:
- `CODEAGENT.md` - Team-shared (checked into git)
- `.claude.local.md` - Personal/local only (gitignored)

## Step 3: Draft Additions

**Keep it concise** - one line per concept. CODEAGENT.md is part of the prompt, so brevity matters.

Format: `<command or pattern>` - `<brief description>`

Avoid:
- Verbose explanations
- Obvious information
- One-off fixes unlikely to recur

## Step 4: Show Proposed Changes

For each addition:

```
### Update: ./CODEAGENT.md

**Why:** [one-line reason]

\`\`\`diff
+ [the addition - keep it brief]
\`\`\`
```

## Step 5: Apply with Approval

Ask if the user wants to apply the changes. Only edit files they approve.
