# Advanced Patterns: Hooks, Orchestration, and CI/CD

Beyond the basics. These patterns are for teams and solo devs who want production-grade agent workflows.

---

## 1. Progressive Disclosure — Keep CLAUDE.md Under 60 Lines

Claude's system prompt already consumes ~50 instructions. You have a budget of roughly **100-150 additional instructions** before quality degrades — and the degradation is **uniform** across all instructions, not just the new ones.

### The Three-Layer Architecture

```
Layer 1: CLAUDE.md (always loaded, <60 lines)
   → Project identity, tech stack, critical commands, pointers

Layer 2: .claude/rules/*.md (auto-loaded, scoped by file path)
   → Conditional rules that activate only when Claude works on matching files

Layer 3: Skills & Agent Guides (on-demand)
   → Deep reference loaded only when invoked
```

### Path-Scoped Rules

Create `.claude/rules/` directory with markdown files:

```markdown
# .claude/rules/frontend-testing.md
---
paths:
  - "src/components/**"
  - "src/app/**"
---

# Frontend Testing Rules
- Use Vitest with React Testing Library
- Test user behavior, not implementation details
- Every component needs at minimum a render test
```

Rules **without** a `paths` field load unconditionally. Rules **with** `paths` only activate when Claude works on matching files. This keeps context lean.

### The @import Syntax

Reference deep documentation without embedding it:

```markdown
# CLAUDE.md
When working on API routes, read @docs/api-patterns.md first.
When working on database changes, read @docs/schema-guide.md first.
```

**Don't embed everything — pitch Claude on when to read it.**

Sources: [alexop.dev - Stop Bloating Your CLAUDE.md](https://alexop.dev/posts/stop-bloating-your-claude-md-progressive-disclosure-ai-coding-tools/), [groff.dev - 3-Tier Architecture](https://www.groff.dev/blog/implementing-claude-md-agent-skills)

---

## 2. Hooks — Deterministic Enforcement

### Why Hooks > Instructions

From Trail of Bits: "An instruction saying 'never use rm -rf' can be forgotten under context pressure. A hook fires every single time."

Hooks are shell commands that run at specific lifecycle events. There are **12 hook event types**:

| Event | When It Fires |
|-------|--------------|
| PreToolUse | Before any tool executes (block, modify, or log) |
| PostToolUse | After tool execution (log results, detect errors) |
| SessionStart / SessionEnd | Session lifecycle |
| SubagentStart / SubagentStop | Track subagent behavior |
| UserPromptSubmit | When user sends a prompt |
| Notification | User interaction events |
| Stop | Session completion |
| PreCompact | Before context compaction |
| PermissionRequest | Permission prompt events |
| PostToolUseFailure | Tool execution failures |

### Security Hooks — Block Dangerous Commands

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "description": "Block rm -rf",
        "command": "echo '$TOOL_INPUT' | jq -r '.command' | grep -qE 'rm\\s+-rf' && echo 'BLOCKED: Use trash instead of rm -rf' >&2 && exit 2 || exit 0"
      },
      {
        "matcher": "Bash",
        "description": "Block force push to main",
        "command": "echo '$TOOL_INPUT' | jq -r '.command' | grep -qE 'git\\s+push.*--force.*\\b(main|master)\\b' && echo 'BLOCKED: Never force push to main' >&2 && exit 2 || exit 0"
      },
      {
        "matcher": "Bash",
        "description": "Block database migrations",
        "command": "echo '$TOOL_INPUT' | jq -r '.command' | grep -qE '(prisma\\s+db\\s+push|prisma\\s+migrate\\s+deploy|knex\\s+migrate)' && echo 'BLOCKED: Database migrations require human approval' >&2 && exit 2 || exit 0"
      }
    ]
  }
}
```

**Exit codes:**
- `0` = allow (continue execution)
- `2` = block (prevent execution, show message)

### Input Modification — Rewrite Commands Before Execution

PreToolUse hooks can **modify** tool inputs via `updatedInput`:

```json
{
  "hookSpecificOutput": {
    "hookEventName": "PreToolUse",
    "permissionDecision": "allow",
    "updatedInput": {
      "command": "rm -i /path/to/dir"
    }
  }
}
```

This rewrites `rm -rf` to `rm -i` (interactive mode) before execution — deterministic, no extra LLM turn needed.

### Auto-Format After Writes

```json
{
  "PostToolUse": [
    {
      "matcher": "Write",
      "description": "Auto-format written files",
      "command": "npx prettier --write \"$(echo '$TOOL_INPUT' | jq -r '.file_path')\" 2>/dev/null || true"
    }
  ]
}
```

### Audit Logging

```json
{
  "PostToolUse": [
    {
      "matcher": "Bash",
      "description": "Log all bash commands",
      "command": "echo \"$(date -u +%Y-%m-%dT%H:%M:%SZ) | $TOOL_INPUT\" >> ~/.claude/bash-audit.log"
    }
  ]
}
```

Sources: [Trail of Bits claude-code-config](https://github.com/trailofbits/claude-code-config), [Codacy - Deterministic Guardrails](https://blog.codacy.com/equipping-claude-code-with-deterministic-security-guardrails), [Claude Code Hooks Docs](https://code.claude.com/docs/en/hooks)

---

## 3. Headless Mode — Agents Without Human Interaction

The `-p` (print) flag runs Claude Code non-interactively:

```bash
# Basic headless execution
claude -p "Add a health check endpoint at /api/health" \
  --allowedTools "Read,Edit,Write,Bash(npm test)"

# JSON output for pipeline integration
claude -p "Fix the failing tests" \
  --allowedTools "Read,Edit,Bash(npm test *)" \
  --output-format json

# Streaming output for real-time monitoring
claude -p "Implement feature per spec.md" \
  --allowedTools "Read,Edit,Write,Bash(git *),Bash(npm *)" \
  --output-format stream-json \
  | tee claude_output.jsonl
```

**Key:** `--allowedTools` pre-authorizes specific tools so the agent never blocks waiting for approval. Scope with globs: `Bash(git diff *)` allows any `git diff` subcommand.

**Output formats:**
- `text` — plain text (default)
- `json` — structured with `total_cost_usd`, `duration_ms`, `num_turns`, `session_id`
- `stream-json` — newline-delimited JSON for real-time streaming

---

## 4. CI/CD — Claude Code in GitHub Actions

Anthropic ships an official GitHub Action:

```yaml
name: Claude Code Review
on:
  pull_request:
  issue_comment:
    types: [created]

jobs:
  claude:
    runs-on: ubuntu-latest
    steps:
      - uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          trigger_phrase: "@claude"
```

**Use cases in production:**
- Automatic PR code review against CLAUDE.md checklist
- Issue triage and labeling
- Documentation sync when API routes change
- Security-focused reviews on auth/payment file changes

Source: [anthropics/claude-code-action](https://github.com/anthropics/claude-code-action)

---

## 5. Agent Teams — Parallel Execution

### Git Worktrees (Standard Isolation)

```bash
# Create isolated worktrees for parallel agents
git worktree add .worktrees/feature-auth feature/auth
git worktree add .worktrees/feature-dashboard feature/dashboard

# Run agents in parallel
claude -p "Implement auth flow" --cwd .worktrees/feature-auth &
claude -p "Build dashboard" --cwd .worktrees/feature-dashboard &
wait

# Clean up
git worktree remove .worktrees/feature-auth
git worktree remove .worktrees/feature-dashboard
```

**Why worktrees over clones:** Shared git objects (no duplicate storage), each has its own branch and file state, merges are clean git operations.

### Claude Code Swarms (Experimental)

Claude Code supports native multi-agent teams:

```json
{ "env": { "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1" } }
```

Pattern: **Plan in plan mode (cheap), then hand the plan to a team for parallel execution (expensive but fast).**

### Cost Optimization — Model Tiering

Use capable models to plan, cheaper models to execute:

```
Opus/Sonnet for planning → Haiku for execution
```

This reduces costs by up to 90% compared to using frontier models for everything.

**Average costs:**
- ~$6/developer/day (90% of users under $12)
- ~$100-200/month with Sonnet 4.5

Sources: [Anthropic - Agent Teams](https://code.claude.com/docs/en/agent-teams), [Simon Willison - Parallel Coding Agents](https://simonwillison.net/2025/Oct/5/parallel-coding-agents/), [Claude Code Cost Management](https://code.claude.com/docs/en/costs)

---

## 6. Long-Running Agents — The Two-Phase Harness

Anthropic's recommended pattern for complex, multi-session work:

### Phase 1: Initializer Agent (Session 1)
- Reads the specification
- Creates `feature_list.json` with granular test cases
- Sets up project structure
- Creates `claude-progress.txt` — structured state file
- Makes initial git commit as baseline

### Phase 2: Coding Agent (Sessions 2+)
- Reads git log + `claude-progress.txt` to understand current state
- Picks **ONE** feature to implement (not many)
- Runs tests to verify
- Commits with clear message
- Updates `claude-progress.txt`
- Leaves clean artifacts for the next session

**The key insight:** `claude-progress.txt` is not a conversation log. It's a structured record of what was attempted, what succeeded, what failed, and what's next. It gives any new session enough context to resume immediately.

Source: [Anthropic - Effective Harnesses for Long-Running Agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)

---

## 7. Custom Subagents

Define specialist agents as markdown files in `.claude/agents/`:

```markdown
# .claude/agents/debugger.md
---
name: debugger
description: Methodically troubleshoot issues. Use when facing bugs, errors, or unexpected behavior.
model: sonnet
tools: [read, grep, bash]
---

You are a methodical debugger. When given a bug report:

1. Reproduce the issue first
2. Read error logs and stack traces
3. Form a hypothesis
4. Test with minimal changes
5. Never make speculative fixes without evidence
```

Claude Code suggests invoking agents based on the `description` field. The `model` field enables cost optimization (use `haiku` for simple tasks).

Source: [Claude Code Subagents](https://code.claude.com/docs/en/sub-agents)

---

## 8. Error Recovery Patterns

### Classified Retry
Separate retriable (503, rate limits, network) from non-retriable (auth, invalid input) errors. Only retry what makes sense.

### Self-Correction
Agents can observe incorrect output and change their approach — not just retry the same operation. This is fundamentally different from traditional retry logic.

### Escalation Chain
```
Retry (2-3 attempts with backoff)
  → Circuit break (stop retrying)
    → Escalate (ask human or use more capable model)
```

### Checkpoint and Resume
For long-running work, write progress to disk (`claude-progress.txt`) so crashed sessions can resume from a known state.

---

## CLAUDE.md Anti-Patterns

What NOT to do:

| Anti-Pattern | Why It's Bad | Fix |
|---|---|---|
| **Bloated CLAUDE.md** (100+ lines) | Quality degrades uniformly across ALL instructions | Keep under 60 lines; use rules/ and skills |
| **Code style instructions** | "Never send an LLM to do a linter's job" | Use ESLint, Prettier, ruff — enforce via hooks |
| **Over-emphasis** (everything in ALL CAPS) | When everything is critical, nothing is | Save emphasis for genuine safety rules |
| **@-mentioning docs** | Embeds full file into every session's context | Pitch Claude on *when* to read them instead |
| **Duplicating across tiers** | Content drifts apart over time | Single source of truth; skills point to guides |
| **Kitchen sink sessions** | Context fills with irrelevant info | `/clear` between unrelated tasks |

Sources: [HumanLayer - Writing a Good CLAUDE.md](https://www.humanlayer.dev/blog/writing-a-good-claude-md), [rosmur - Claude Code Best Practices](https://rosmur.github.io/claudecode-best-practices/)
