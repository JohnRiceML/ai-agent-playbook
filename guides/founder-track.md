# Founder Track: Managing Your AI Development Team

You don't write code (or you're just getting started). You want to understand what's possible and how to think about AI agents as a team you manage.

---

## The Mental Model: An Org Chart, Not a Tool

Stop thinking of AI as a tool you use. Think of it as a **team you manage**.

```
You (CEO / Product Owner)
  │
  ├── Product Director Agent    → Turns your ideas into specs
  ├── Execute Agent             → Builds the features
  ├── Code Reviewer Agent       → Checks quality and safety
  ├── QA Auditor Agent          → Verifies consistency
  ├── Pipeline Medic Agent      → Monitors reliability
  └── UX Director Agent         → Ensures design consistency
```

You don't need to know how to code to manage this team. You need to know:
1. **What you want built** (product vision)
2. **What "done" looks like** (acceptance criteria)
3. **What's off-limits** (guardrails)
4. **When to approve** (review the results)

---

## How Requests Flow: From Idea to Shipped Feature

### Step 1: You Write a Ticket (Plain English)
On a Notion board (or any project board), write what you want:

```
Title: Add a health check endpoint
Description: I need a simple API endpoint that returns whether
the system is running. It should return the current time and
a status of "ok" or "error".
```

That's it. No code. No technical details. Just what you want.

### Step 2: AI Reviews the Ticket
An AI "review agent" reads your ticket, explores the codebase, and tells you:

```
Ease: 9/10 (single file, < 20 lines)
Confidence: 10/10 (trivial change, zero ambiguity)
Estimated cost: $0.35
Affected files: 1
```

If the ease or confidence is low, you can add more detail to the ticket or decide it's not worth doing yet.

### Step 3: AI Implements It
An AI "execute agent" builds the feature:
- Creates a separate branch (doesn't touch the live product)
- Writes the code following the project's rules
- Writes tests to verify it works
- Runs the tests to confirm
- Creates a Pull Request for review

### Step 4: You Review (or Your Dev Reviews)
The PR shows exactly what changed. You or a developer reviews it:
- Does it do what the ticket asked?
- Do the tests pass?
- Does it look reasonable?

Approve → it merges. Request changes → the agent tries again.

### Step 5: It Ships
Standard deployment. The feature is live.

**Total human effort:** Write a ticket (2 min) + review a PR (5 min) = **7 minutes**.

---

## What It Costs

### Per Feature (AI Agent Costs)

| Complexity | Example | Typical Cost |
|-----------|---------|-------------|
| Trivial | Add a status endpoint | $0.35 - $0.75 |
| Simple | Add sorting to a list | $0.50 - $1.35 |
| Medium | Add a new page with API | $0.75 - $2.50 |
| Complex | New feature with multiple pages | $2.30 - $8.50 |

### vs. Hiring

| | AI Agent Team | Junior Developer | Senior Developer |
|--|-------------|-----------------|-----------------|
| Monthly cost | $100-300 (usage-based) | $5,000-8,000 | $10,000-15,000 |
| Available | 24/7 | Business hours | Business hours |
| Ramp-up time | Immediate | 2-4 weeks | 1-2 weeks |
| Consistent quality | Follows rules every time | Varies by day | Varies by day |
| Scales to 10x | Add more agents | Hire more people | Hire more people |

**Important caveat:** AI agents can't replace product thinking, user research, or strategic decisions. They replace implementation work. You still need someone (you!) deciding *what* to build.

---

## The Employee Handbook (CLAUDE.md)

Every team needs a handbook. For AI agents, it's a file called `CLAUDE.md` in your project.

Think of it as your company's policies:

| Company Handbook | CLAUDE.md |
|------------------|-----------|
| "Never share customer data" | "Never expose user emails in API responses" |
| "All expenses need manager approval" | "Never install new dependencies without asking" |
| "Follow the brand guidelines" | "Use the design system colors and spacing" |
| "Report incidents immediately" | "Log errors with context, never silently fail" |
| "Security training required" | "All database queries must be scoped by tenant ID" |

You can write these rules in plain English. The AI reads them before every task.

**The best part:** Unlike human employees, AI agents read the handbook every single time. They never forget a rule, skip a step, or ignore a policy.

---

## What You Review vs. What Agents Do

### You Do:
- Decide **what** to build (product vision, user needs)
- Write tickets describing what you want
- Set the rules (CLAUDE.md — the handbook)
- Review the output (approve or request changes)
- Make business decisions (pricing, features, priorities)

### Agents Do:
- Figure out **how** to build it
- Write the code
- Write the tests
- Verify the code follows the rules
- Create the PR for your review
- Check each other's work

---

## Guardrails: Keeping Agents Safe

Just like you'd set policies for employees, you set guardrails for agents:

### 1. What They Can't Touch
Certain files are off-limits:
- Database schema (structural changes need human approval)
- Payment code (financial operations are too risky)
- Environment files (contain secrets and API keys)

### 2. What They Must Do Before Shipping
- Code must compile (no syntax errors)
- Tests must pass (behavior is verified)
- Build must succeed (works in production mode)

### 3. Budget Limits
- Review phase: max $2 per ticket
- Implementation phase: max $15 per ticket
- If an agent exceeds the budget, it stops and asks for help

### 4. Human Approval Required
No code reaches production without a human approving it. This is the most important guardrail.

---

## Getting Started Without Coding

### Option 1: Use Claude Code Directly
```bash
npm install -g @anthropic-ai/claude-code
cd ~/your-project
claude
```

Then talk to it in plain English:
```
> Look at this project and tell me what it does.
> Add a page that shows a list of our products.
> Write tests for the new page.
```

### Option 2: Use TicketToPR for Full Automation
If you have a developer set up [TicketToPR](https://www.npmjs.com/package/ticket-to-pr), you can:
1. Write tickets on a Notion board
2. Drag them to "Review" → AI scores them
3. Drag scored tickets to "Execute" → AI builds them
4. Review the PR on GitHub → Approve or request changes

**No terminal required.** You work in Notion and GitHub.

### Option 3: Pair with a Developer
Have a developer set up the CLAUDE.md and agent specs. Then you manage the Notion board and review process. The developer handles complex architecture decisions; agents handle routine implementation.

---

## Real Numbers from Real Projects

### Peekaboo (Brand Visibility Platform)
- **1,162 commits** in the codebase
- **13 specialist agents** handling different domains
- **5 AI models** queried in parallel for brand analysis
- Built by one developer + AI agent team
- Production users paying for the service

### Local Visibility Suite
- **44,804 lines of code** across 6 complete tools
- **24 API endpoints**, 8 test suites
- Real Google and Yelp API integrations
- PDF report generation + CRM integration
- Built in ~2-3 months by one person + agents

### What This Would Have Taken Without Agents
A project of Peekaboo's complexity (1,162 commits, 13 specialized systems, 5 API integrations) would typically require a team of 3-5 developers working 4-6 months. With AI agents, it's one person moving at that team's pace.

---

## The Honest Truth

### What AI Agents Are Great At:
- Implementing well-defined features
- Writing boilerplate code
- Following established patterns
- Writing tests
- Catching consistency issues
- Working 24/7 without burnout

### What AI Agents Are Bad At:
- Deciding what to build (that's your job)
- Understanding your users (that's your job)
- Making business strategy decisions (that's your job)
- Handling truly novel problems with no precedent
- Knowing when something "feels wrong" to a user

### The Bottom Line
AI agents replace implementation work, not product thinking. If you know what to build and can describe it clearly, AI agents can help you build it at a fraction of the traditional cost and timeline.

The question isn't "Will AI replace developers?" It's "How do I best leverage AI to ship what my users need?"
