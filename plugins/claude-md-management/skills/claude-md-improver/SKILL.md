---
name: claude-md-improver
description: Audit and improve CODEAGENT.md files in repositories. Use when user asks to check, audit, update, improve, or fix CODEAGENT.md files. Scans for all CODEAGENT.md files, evaluates quality against templates, outputs quality report, then makes targeted updates. Also use when the user mentions "CODEAGENT.md maintenance" or "project memory optimization".
tools: Read, Glob, Grep, Bash, Edit
---

# CODEAGENT.md Improver

Audit, evaluate, and improve CODEAGENT.md files across a codebase to ensure CodeAgent has optimal project context.

**This skill can write to CODEAGENT.md files.** After presenting a quality report and getting user approval, it updates CODEAGENT.md files with targeted improvements.

## Workflow

### Phase 1: Discovery

Find all CODEAGENT.md files in the repository:

```bash
find . -name "CODEAGENT.md" -o -name ".claude.md" -o -name ".claude.local.md" 2>/dev/null | head -50
```

**File Types & Locations:**

| Type | Location | Purpose |
|------|----------|---------|
| Project root | `./CODEAGENT.md` | Primary project context (checked into git, shared with team) |
| Local overrides | `./.claude.local.md` | Personal/local settings (gitignored, not shared) |
| Global defaults | `~/.codeAgent/CODEAGENT.md` | User-wide defaults across all projects |
| Package-specific | `./packages/*/CODEAGENT.md` | Module-level context in monorepos |
| Subdirectory | Any nested location | Feature/domain-specific context |

**Note:** the agent auto-discovers CODEAGENT.md files in parent directories, making monorepo setups work automatically.

### Phase 2: Quality Assessment

For each CODEAGENT.md file, evaluate against quality criteria. See [references/quality-criteria.md](references/quality-criteria.md) for detailed rubrics.

**Quick Assessment Checklist:**

| Criterion | Weight | Check |
|-----------|--------|-------|
| Commands/workflows documented | High | Are build/test/deploy commands present? |
| Architecture clarity | High | Can the agent understand the codebase structure? |
| Non-obvious patterns | Medium | Are gotchas and quirks documented? |
| Conciseness | Medium | No verbose explanations or obvious info? |
| Currency | High | Does it reflect current codebase state? |
| Actionability | High | Are instructions executable, not vague? |

**Quality Scores:**
- **A (90-100)**: Comprehensive, current, actionable
- **B (70-89)**: Good coverage, minor gaps
- **C (50-69)**: Basic info, missing key sections
- **D (30-49)**: Sparse or outdated
- **F (0-29)**: Missing or severely outdated

### Phase 3: Quality Report Output

**ALWAYS output the quality report BEFORE making any updates.**

Format:

```
## CODEAGENT.md Quality Report

### Summary
- Files found: X
- Average score: X/100
- Files needing update: X

### File-by-File Assessment

#### 1. ./CODEAGENT.md (Project Root)
**Score: XX/100 (Grade: X)**

| Criterion | Score | Notes |
|-----------|-------|-------|
| Commands/workflows | X/20 | ... |
| Architecture clarity | X/20 | ... |
| Non-obvious patterns | X/15 | ... |
| Conciseness | X/15 | ... |
| Currency | X/15 | ... |
| Actionability | X/15 | ... |

**Issues:**
- [List specific problems]

**Recommended additions:**
- [List what should be added]

#### 2. ./packages/api/CODEAGENT.md (Package-specific)
...
```

### Phase 4: Targeted Updates

After outputting the quality report, ask user for confirmation before updating.

**Update Guidelines (Critical):**

1. **Propose targeted additions only** - Focus on genuinely useful info:
   - Commands or workflows discovered during analysis
   - Gotchas or non-obvious patterns found in code
   - Package relationships that weren't clear
   - Testing approaches that work
   - Configuration quirks

2. **Keep it minimal** - Avoid:
   - Restating what's obvious from the code
   - Generic best practices already covered
   - One-off fixes unlikely to recur
   - Verbose explanations when a one-liner suffices

3. **Show diffs** - For each change, show:
   - Which CODEAGENT.md file to update
   - The specific addition (as a diff or quoted block)
   - Brief explanation of why this helps future sessions

**Diff Format:**

```markdown
### Update: ./CODEAGENT.md

**Why:** Build command was missing, causing confusion about how to run the project.

```diff
+ ## Quick Start
+
+ ```bash
+ npm install
+ npm run dev  # Start development server on port 3000
+ ```
```
```

### Phase 5: Apply Updates

After user approval, apply changes using the str_replace tool. Preserve existing content structure.

## Templates

See [references/templates.md](references/templates.md) for CODEAGENT.md templates by project type.

## Common Issues to Flag

1. **Stale commands**: Build commands that no longer work
2. **Missing dependencies**: Required tools not mentioned
3. **Outdated architecture**: File structure that's changed
4. **Missing environment setup**: Required env vars or config
5. **Broken test commands**: Test scripts that have changed
6. **Undocumented gotchas**: Non-obvious patterns not captured

## User Tips to Share

When presenting recommendations, remind users:

- **`#` key shortcut**: During a the agent session, press `#` to have the agent auto-incorporate learnings into CODEAGENT.md
- **Keep it concise**: CODEAGENT.md should be human-readable; dense is better than verbose
- **Actionable commands**: All documented commands should be copy-paste ready
- **Use `.claude.local.md`**: For personal preferences not shared with team (add to `.gitignore`)
- **Global defaults**: Put user-wide preferences in `~/.codeAgent/CODEAGENT.md`

## What Makes a Great CODEAGENT.md

**Key principles:**
- Concise and human-readable
- Actionable commands that can be copy-pasted
- Project-specific patterns, not generic advice
- Non-obvious gotchas and warnings

**Recommended sections** (use only what's relevant):
- Commands (build, test, dev, lint)
- Architecture (directory structure)
- Key Files (entry points, config)
- Code Style (project conventions)
- Environment (required vars, setup)
- Testing (commands, patterns)
- Gotchas (quirks, common mistakes)
- Workflow (when to do what)
