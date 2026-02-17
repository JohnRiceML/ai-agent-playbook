# Developer Track: Engineering with AI Agents

You write code. You care about quality. You're skeptical of "AI writes all the code" hype. Good — this guide is for you.

---

## The Thesis

You are not replacing yourself. You are **changing what you spend time on**:

| Before | After |
|--------|-------|
| Writing implementation code | Writing agent specs and CLAUDE.md rules |
| Debugging your own bugs | Reviewing agent-generated PRs |
| Context-switching between tasks | Orchestrating parallel agents |
| Googling API docs | Grounding agents with real benchmarks |
| Writing tests after the fact | Defining test requirements upfront |

**Your job got more senior, not smaller.** You're now the architect, code reviewer, and team lead.

---

## The Agent Team Architecture

Think of agents like a small engineering team you manage:

```
You (Architect / Tech Lead)
  |
  |-- Execute Agent        → Implements features (your "junior dev")
  |-- Scoring Auditor      → Verifies mathematical consistency (your "QA")
  |-- Pipeline Medic       → Audits reliability and resilience (your "SRE")
  |-- Code Reviewer        → Reviews backend safety, performance (your "senior dev")
  |-- Lead Architect       → Enforces separation of concerns (your "staff eng")
```

Each agent has:
- A **role** (single responsibility)
- **Tools** (scoped access — read-only vs. read-write)
- **Context** (the specific files, formulas, and benchmarks it needs)
- **Output format** (structured, predictable reports)
- **Constraints** (what it must never do)

---

## Pattern 1: The CLAUDE.md Constitution

Your `CLAUDE.md` is the most important file in your project. It's the document every agent reads before doing anything.

**Structure it like this:**

```
1. CRITICAL RULES (what to NEVER do — lead with danger)
2. PROJECT OVERVIEW (tech stack, architecture, one paragraph)
3. COMMANDS (build, test, lint — exact commands)
4. CODE CONVENTIONS (only what differs from defaults)
5. TESTING REQUIREMENTS (what must be tested, how)
6. DEFINITION OF DONE (the checklist)
7. WORKING WITH THIS CODEBASE (gotchas, patterns, source-of-truth locations)
8. FILE RESTRICTIONS (what agents can't touch)
```

**Key insight from Trail of Bits:** "An instruction saying 'never use rm -rf' can be forgotten under context pressure. A hook fires every single time." Use CLAUDE.md for guidance. Use hooks for enforcement.

---

## Pattern 2: Agent-Reviews-Agent

The most powerful pattern is having agents review each other's work:

```
Step 1: Execute Agent implements a feature
Step 2: Code Reviewer agent reviews the implementation
Step 3: Scoring Auditor verifies formula consistency
Step 4: You review the PR (informed by agent reports)
```

**How to set this up:**

1. Create specialist agent specs in `.claude/agents/`
2. After the execute agent finishes, invoke the reviewers:
   ```
   > Use the code-reviewer agent to review the changes in the last commit
   ```
3. The reviewer produces a structured report
4. You read the report and the diff together

**Why this works:** It's the same principle as code review on human teams. The implementer has blind spots. The reviewer catches them. The difference is that agents actually read every line.

---

## Pattern 3: Testing-First with Agents

Don't let agents write code without tests. Build the expectation into your workflow:

### The conversation pattern:
```
> Add a function that calculates visibility scores from raw mention data.
> Write the test first, then the implementation.
> The test should cover: happy path, zero mentions, all mentions failed, single mention.
```

### The Definition of Done enforcement:
Add this to your CLAUDE.md:
```markdown
## Testing Requirements
- Every function in lib/ must have a corresponding .test.ts file
- Write the test FIRST, then the implementation
- Tests must cover: happy path, edge cases, error conditions
- Run `npm test` before committing — zero failures required
```

### The agent-validates-agent pattern:
After an execute agent builds a feature, spin up a second conversation:
```
> Read the changes in the last commit. Are the tests actually testing
> the right things? Do they cover edge cases? What's missing?
```

This second agent has no ego invested in the code. It will find gaps.

---

## Pattern 4: Grounding with Real Data

Agents make bad decisions when they guess. They make great decisions when you give them data.

### Bad: Vague instructions
```
Handle API timeouts appropriately.
```

### Good: Data-grounded instructions
```
## API Timeout Configuration

| Endpoint | P90 Latency | Soft Timeout | Hard Timeout |
|----------|-------------|--------------|-------------|
| ChatGPT  | 58s         | 70s          | 90s         |
| Gemini   | 29s         | 35s          | 45s         |

Rules:
- Soft timeout = P90 + 20% buffer
- Hard timeout = soft timeout + 20s
- Use AbortController, not Promise.race
- Never set timeouts based on averages
```

### What to ground:
- **Formulas** with worked examples
- **Latency benchmarks** with P90 (not averages)
- **File paths** to sources of truth
- **Verification logs** showing what correct output looks like
- **Cost data** (API costs per call, token budgets)

---

## Pattern 5: The TicketToPR Orchestration

For fully automated ticket-to-PR workflows:

```bash
npm install -g ticket-to-pr
ticket-to-pr init    # Guided setup (Notion board + project config)
ticket-to-pr doctor  # Verify everything is connected
ticket-to-pr --once  # Process one batch of tickets
```

**The flow:**
```
Notion "Review" column → AI reads codebase, scores feasibility (1-10)
                        → Ticket moves to "Scored"
                        → You decide if it's worth implementing
Notion "Execute" column → AI creates branch, implements, commits
                        → Builds must pass
                        → Blocked files can't be touched
                        → PR appears on GitHub
                        → You review and merge
```

**Safety built in:**
- Isolated git worktrees (agents never touch main)
- Build validation gate (broken code can't push)
- Blocked files gate (schema, migrations, etc.)
- Budget caps ($2 review, $15 execute)
- Human review required (no auto-merge)

**Cost per ticket:** $0.35-$0.55 for simple tasks. $2-5 for complex features.

---

## Pattern 6: The Feedback Loop

When an agent makes a mistake, it becomes a permanent learning:

```
1. Agent generates code with an N+1 query
2. Code reviewer catches it (or you do)
3. You add to CLAUDE.md:
   "Never put database queries inside loops. Use includes/joins."
4. Every future agent session reads this rule
5. It never happens again
```

**This is your superpower.** Junior devs make the same mistakes repeatedly because they forget feedback. Agents read the CLAUDE.md every single time.

Over time, your CLAUDE.md becomes a distillation of every lesson learned — a living document that makes every agent better.

---

## Anti-Pattern: Vibe Coding

What you're doing is **not** vibe coding. Here's the difference:

| Vibe Coding | Agent Engineering |
|-------------|-------------------|
| "Just make it work" | CLAUDE.md constitution with rules |
| No tests | Testing-first, agent-reviews-agent |
| No architecture | SOLID principles, separation of concerns |
| No review | Human reviews every PR |
| "I'll fix it later" | Definition of Done enforced |
| 2.74x more security vulns | Hooks + blocked files + build gates |

AI-generated code has 1.7x more major issues when unreviewed (The New Stack, 2026). Your review process is what turns AI output into production code.

---

## Getting Started Checklist

- [ ] Create a `CLAUDE.md` in your project (use the [template](../templates/CLAUDE.md.template))
- [ ] Add your critical "NEVER" rules
- [ ] Add your build/test/lint commands
- [ ] Define your Definition of Done
- [ ] Create one specialist agent spec (start with code-reviewer)
- [ ] Try the agent-reviews-agent pattern on your next PR
- [ ] Install hooks for audit logging (see [hooks template](../templates/hooks.example.json))
- [ ] Consider [TicketToPR](https://www.npmjs.com/package/ticket-to-pr) for full automation
