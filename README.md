# AI Agent Playbook

A practical guide to building with AI developer agents — from your first CLAUDE.md to a full agent team.

**Not vibe coding.** This is engineering with AI agents: guardrails, tests, architecture, and human review at every step.

## Who This Is For

### Developers (The Dev Track)
You write code. You want to ship faster without sacrificing quality. You'll learn how to configure AI agents with SOLID principles, testing-first workflows, and agent-reviews-agent patterns. Start with the [Dev Track Guide](guides/dev-track.md).

### Founders & Builders (The Founder Track)
You don't write code (or you're code-curious). You want to understand what's possible and how to think about AI agents as a team you manage. Start with the [Founder Track Guide](guides/founder-track.md).

## What's Inside

```
ai-agent-playbook/
  templates/
    CLAUDE.md.template       # Your first agent constitution — copy and customize
    agent-spec.template.md   # Template for creating specialist agents
    hooks.example.json       # Security blocks + audit logging + auto-format hooks
  guides/
    core-principles.md       # KISS, SOLID, Testing, DoD — with hard data
    getting-started.md       # Your first 30 minutes with Claude Code
    dev-track.md             # For developers: agent-reviews-agent, TicketToPR
    founder-track.md         # For non-coders: the AI team mental model
    advanced-patterns.md     # Hooks, CI/CD, headless mode, agent teams, orchestration
  examples/
    scoring-auditor.md       # Real agent spec: mathematical consistency checker
    pipeline-medic.md        # Real agent spec: reliability & resilience auditor
    code-reviewer.md         # Real agent spec: backend code review specialist
```

## Core Philosophy

### 1. KISS — Keep It Simple
Agents work best with clear, simple instructions. One agent, one job. Don't build a "do everything" agent — build a team of specialists.

### 2. SOLID — Applied to Agents
- **S**ingle Responsibility: Each agent owns one domain (scoring, pipeline health, code review)
- **O**pen/Closed: Agent specs are extensible (add rules) without modifying the core
- **L**iskov: Any agent following the spec template produces predictable output
- **I**nterface Segregation: Agents only get the tools they need (read-only vs. write access)
- **D**ependency Inversion: Agents depend on your CLAUDE.md (the constitution), not on each other

### 3. Testing, Testing, Testing
Don't assume AI-generated code works. Write a simple test for anything you can. Have the system spin up a new agent to review what another agent built. Build from there.

### 4. Definition of Done
AI code is "done" when:
- Types pass (`tsc --noEmit`)
- Tests pass (`vitest run` / `jest`)
- Linter passes
- You can explain *why* the code works, not just *that* it works
- It fits the existing architecture
- No unexplained dependencies were added

### 5. Ground Everything in Real Data
Don't let agents guess. Give them benchmarks, formulas, file paths, and examples. An agent with real P90 latency data makes better timeout decisions than one told to "set reasonable timeouts."

## Quick Start

```bash
# 1. Install Claude Code (if you haven't)
npm install -g @anthropic-ai/claude-code

# 2. Copy the starter CLAUDE.md into your project
cp templates/CLAUDE.md.template ~/your-project/CLAUDE.md

# 3. Customize it for your project (open in your editor and follow the comments)

# 4. Start Claude Code in your project
cd ~/your-project
claude
```

That's it. You now have an AI agent that knows your project's rules.

## The Agentic Stack (What Powers This)

This playbook is based on real production experience shipping multiple SaaS products with AI agents:

| Layer | Tool | What It Does |
|-------|------|-------------|
| **Constitution** | `CLAUDE.md` | Project rules, architecture, guardrails |
| **Agent Specs** | `.claude/agents/*.md` | Specialist agent definitions |
| **Orchestration** | [TicketToPR](https://www.npmjs.com/package/ticket-to-pr) | Notion ticket → AI review → AI implementation → PR |
| **Enforcement** | Hooks + Permissions | Log every action, block dangerous commands |
| **Validation** | Tests + Build Gates | Code must pass before it ships |
| **Human Review** | GitHub PRs | You review every change before merge |

## From the Talk

This repo accompanies the talk **"Building with a Team of AI Developer Agents"** at [No-Code Coffee](https://www.nocodecoffee.com/) (Feb 19, 2026).

**[View the slide deck](https://johnriceml.github.io/ai-agent-playbook/talk/slides.html)**

Key takeaway: AI solves the *how*, not the *what*. Your job is knowing what to build, how to architect it, and what "done" looks like. The agents handle implementation. Your job got more interesting, not less.

## Resources

See [talk/resources.md](talk/resources.md) for the full list of research, articles, and tools referenced.

## License

MIT — use this however you want.
