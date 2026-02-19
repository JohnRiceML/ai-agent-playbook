# Talk Outline: Developer Track

**Title:** Building with a Team of AI Developer Agents: From Solo Dev to Scalable Output
**Duration:** ~45 min presentation + Q&A
**Audience:** Full-stack engineers, product engineers, AI-curious developers

---

## Opening (5 min) — Shared with Both Tracks

### The Hook
"I shipped 44,000 lines of code across 6 tools in about 2 months. Solo. I have another product — Peekaboo — with 1,162 commits and 13 specialist AI agents. One person.

But here's the thing — I'm not vibe coding. There's an industry report showing AI co-authored code has 1.7x more major issues and 2.74x more security vulnerabilities when unreviewed. I review everything. I have guardrails, tests, and a Definition of Done. Let me show you how."

### Audience Poll
"Quick show of hands — who here writes code? Who manages developers? Who has never opened a terminal?"

→ Fork: "Developers, this is your track. I'm going deep on architecture, testing, and the agent-reviews-agent pattern."

---

## Section 1: My Agentic Stack (10 min)

### The TicketToPR Demo
**Show:** Notion board with 7 columns (Backlog → Review → Scored → Execute → In Progress → PR Ready → Failed)

**Walk through a real ticket:**
1. Ticket in "Review" column: "Add sort by visibility score to prompt list"
2. Review agent reads the codebase, produces:
   - Ease: 8/10, Confidence: 9/10
   - Spec: step-by-step implementation plan
   - Affected files: 3 files
   - Cost: $0.42
3. I decide it's worth implementing → drag to "Execute"
4. Execute agent:
   - Creates isolated worktree (`notion/30adc119/add-sort-by-visibility...`)
   - Implements the feature
   - Runs build → passes
   - Pushes branch, creates PR
5. I review the PR on GitHub

**Key stat:** $0.35-$0.55 per simple feature. $2-5 for complex ones.

### The Safety Architecture
```
Permissions (whitelist-based) → what agents CAN do
CLAUDE.md (constitution)      → what agents SHOULD do
Hooks (observability)         → what agents DID do
Hard gates (enforcement)      → what agents MUST NOT break
```

**Show:** The actual CLAUDE.md first line:
```
🚨 CRITICAL DATABASE SAFETY RULE 🚨
NEVER run `npx prisma db push` without explicit user permission.
```

---

## Section 2: The Agent Team (10 min)

### Peekaboo's 13 Agents
**Show the org chart slide:**
- Automated Analysis Orchestrator (DevOps)
- Scoring Auditor (QA)
- Pipeline Medic (SRE)
- Cloro Lead (Integration Specialist)
- Lead Backend (Senior Backend Dev)
- Lead Frontend (Senior Frontend Dev)
- Lead Architect (Staff Engineer)
- Product Director (PM)
- UX Style Director (Design Lead)
- Playground Analyst (Data Analyst)
- Source Attribution Auditor (Fact-Checker)
- User Journey Specialist (UX Researcher)
- SEO Specialist (Growth)

### Deep-Dive: The Scoring Auditor
**The story:** "A scoring bug appeared in three UI components. Dashboard showed 78%, detail page showed 44%, API returned 52%. Same entity, same data. Each had subtly re-implemented the formula."

**Show the agent spec:**
- One formula, one source of truth: `lib/scoring/visibility.ts`
- Verification protocol: locate → search → validate → test → report
- Structured output with compliant/non-compliant files
- Grounded in the actual formula with a worked example

**The lesson:** "I didn't need a QA engineer. I needed a QA agent that reads the spec every single time and never forgets the formula."

### Deep-Dive: Pipeline Medic with Real Benchmarks
**Show the benchmark table:**
```
ChatGPT: P90 = 58s
Gemini:  P90 = 29s
Google AIO: P90 = 88s ← the bottleneck
```

"I gave the agent real P90 latency data. Not 'set reasonable timeouts.' The actual numbers. The decision framework: `if timeout > P90 + 20% → CRITICAL`. Agents make great decisions when you give them great data."

---

## Section 3: The Engineering Principles (10 min)

### SOLID Applied to Agents
- **S**ingle Responsibility → one agent, one domain
- **O**pen/Closed → add rules, don't change the role
- **I**nterface Segregation → read-only auditors vs. read-write implementers
- **D**ependency Inversion → agents depend on CLAUDE.md, not each other

"This isn't new. It's the same SOLID principles you already know, applied to a new kind of team member."

### KISS in Practice
**Show two versions of an instruction:**
- Bad: "Review the code and make sure everything is good"
- Good: The scoring-auditor spec with verification protocol, decision framework, structured output

"Simple doesn't mean short. Simple means unambiguous."

### Testing, Testing, Testing
**The pattern:**
```
Execute Agent → writes code + tests
Scoring Auditor → verifies formula consistency
Code Reviewer → checks for N+1 queries, auth gaps
You → reviews the PR with agent reports
```

"Have the system spin up a new agent to review what the endpoint returns. Don't assume — verify. If you can write a test for it, do. The test is the contract between you and the agent."

### Definition of Done
```
Types pass (tsc --noEmit)     → catches type mismatches
Tests pass (vitest run)       → verifies behavior
Build passes (npm run build)  → works in production
You can explain WHY it works  → not just THAT it works
```

"If you can't explain the agent's approach, the code probably has a subtle bug."

---

## Section 4: The Anti-Pattern — Vibe Coding (5 min)

"You've all heard of vibe coding. 'Just tell the AI what you want and ship it.' Here's the data:"

- AI co-authored code: 1.7x more major issues (GitHub PR analysis, Dec 2025)
- Security vulnerabilities: 2.74x higher
- "The vibe coding hangover: three months later, nobody can explain how the code works"

**The contrast:**
```
Vibe Coding          →  Agent Engineering
No constitution      →  38KB CLAUDE.md
No tests             →  Testing-first, agent-reviews-agent
No review            →  Human reviews every PR
No guardrails        →  Hooks + blocked files + build gates
"It works on my machine" → Definition of Done enforced
```

"I'm not vibe coding. I'm engineering with AI agents. The engineering principles don't go away because the author is artificial."

---

## Section 5: The Mindset Shift (5 min)

### What Changed for Me
"I was skeptical. I'm a full-stack engineer. I thought AI coding was for prototypes and demos.

Then I started treating it like a team, not a tool. I write agent specs instead of implementation code. I review PRs instead of writing PRs. I update CLAUDE.md instead of repeating myself in standups.

My job got more interesting. I'm doing architecture, product strategy, and code review — the senior work. The agents handle the implementation — the junior work."

### The Feedback Loop
"When an agent makes a mistake, I don't just fix it. I add a rule to CLAUDE.md. The next time that agent runs, it reads the updated rule. It never makes that mistake again.

Unlike human team members, agents actually read the post-mortem document every single time."

---

## Closing (2 min) — Shared with Both Tracks

### The Free Resource
"Everything I showed you today is in a public repo. Templates, agent specs, guides."

**Show:** `github.com/[your-username]/ai-agent-playbook`
- Starter CLAUDE.md template
- Agent spec templates
- Real examples (scoring auditor, pipeline medic, code reviewer)
- Dev track guide and founder track guide
- Core principles (KISS, SOLID, DoD, Testing)

### One Takeaway
"AI solves the *how*, not the *what*. Your job is knowing what to build. The agents handle implementation. Start with a CLAUDE.md. Add one rule. See what happens."

---

## Q&A Notes

**Likely dev questions:**
- "What about Cursor?" → "Great tool, different philosophy. Cursor puts AI in your editor. Claude Code puts AI in your terminal with autonomous execution. I use CLAUDE.md as the agent constitution; Cursor uses .cursorrules. Same idea, different depth."
- "How do you handle merge conflicts?" → "Isolated worktrees. Each agent works in its own copy. They never touch main."
- "What about complex features that need design decisions?" → "Review phase. The agent scores it first. Low confidence = I need to be more involved. High confidence = let it run."
- "Cost?" → "Claude Pro subscription + token usage. I spend roughly $100-300/month on agent usage across all projects."
