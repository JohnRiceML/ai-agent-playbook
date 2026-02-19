# Talk Outline: Founder / Non-Technical Track

**Title:** Building with a Team of AI Developer Agents: From Solo Dev to Scalable Output
**Duration:** ~45 min presentation + Q&A
**Audience:** Non-technical founders, PMs, no-code/low-code builders

---

## Opening (5 min) — Shared with Both Tracks

### The Hook
"I've launched around five SaaS products. Most recently Peekaboo — a brand visibility platform with real paying customers. My codebase has over 1,100 commits and 13 specialist AI agents.

I don't have a dev team. I have an AI team. And I manage them the same way you'd manage human employees — with a handbook, job descriptions, and performance reviews.

If you're building a product and thinking 'I need to hire developers,' I want to show you what's possible first."

### Audience Poll
"Quick show of hands — who here writes code? Who manages developers? Who has never opened a terminal?"

→ Fork: "Non-coders, this is your track. I'm going to explain this like a business, not like engineering."

---

## Section 1: The AI Team Mental Model (10 min)

### Your Org Chart
"Forget 'AI tool.' Think 'AI employee.' Here's my team for Peekaboo:"

**Show the org chart:**
```
Me (CEO / Product Owner / Architect)
  │
  ├── Product Director      → Turns my ideas into actionable specs
  ├── Execute Agent          → Builds the features
  ├── Code Reviewer          → Checks quality (like a senior dev)
  ├── QA Auditor             → Verifies everything is consistent
  ├── Pipeline Medic         → Monitors system health (like an SRE)
  ├── UX Director            → Ensures design consistency
  ├── Data Analyst           → Research-backed recommendations
  └── 6 more specialists...
```

"Each 'employee' has a job description. One job. Not 'do everything' — one specific responsibility. Just like you'd hire a designer, an accountant, and a salesperson — not one person who does all three."

### The Employee Handbook
"Every team needs rules. Mine are in a file called CLAUDE.md. Think of it as the employee handbook."

**Show side-by-side:**

| Your Company Handbook | My AI Handbook (CLAUDE.md) |
|---|---|
| "Never share customer data" | "Never expose user emails in API responses" |
| "All purchases need approval" | "Never install new software without asking" |
| "Follow the brand guidelines" | "Use the design system colors and spacing" |
| "Report incidents immediately" | "Log all errors with context" |

"The difference? Human employees forget the handbook. AI employees read it every single time."

---

## Section 2: How a Feature Gets Built (10 min)

### The Request Flow
"Let me walk you through exactly what happens when I want a new feature."

**Step 1: I Write a Ticket (2 minutes)**
"In Notion — plain English. Not code. Just what I want."
```
Title: Add sort-by-visibility to the prompt list
Description: Users should be able to sort their prompts
by visibility score, highest to lowest.
```

**Step 2: AI Reviews It (automatic, ~$0.20)**
"My review agent reads the entire codebase, then tells me:"
```
Ease: 8/10 (straightforward change)
Confidence: 9/10 (clear requirements)
Affected files: 3
Estimated cost: $0.42
```

"This is like getting a quote from a contractor before they start work."

**Step 3: I Approve → AI Builds It (automatic, ~$0.35)**
"I drag the ticket to 'Execute.' The AI agent:"
- Creates a safe workspace (doesn't touch the live product)
- Writes the code
- Writes tests to prove it works
- Runs the tests
- Creates a review request

**Step 4: I Review (5 minutes)**
"I look at what changed. Does it do what I asked? Do the tests pass? Approve or send it back."

**Total: 7 minutes of my time. $0.55 in AI costs.**

### The Numbers at Scale
"Last month across all my projects:"
```
Simple features (add a button, fix text):     $0.35-0.75 each
Medium features (new page with data):         $0.75-2.50 each
Complex features (new system with APIs):      $2.30-8.50 each
```

**Compare to hiring:**

| | AI Agent Team | Junior Developer | Agency |
|--|-------------|-----------------|--------|
| Monthly cost | $100-300 | $5,000-8,000 | $10,000-25,000 |
| Available | 24/7 | Business hours | Business hours |
| Ramp-up | Instant | 2-4 weeks | 1-2 weeks |
| Follows rules | Every time | Sometimes | Varies |

---

## Section 3: Guardrails — How I Keep It Safe (8 min)

### The Fear
"The #1 question I get: 'Aren't you scared it'll break something?'"

### The Answer: Four Layers of Safety

**Layer 1: The Handbook (what they should do)**
"My CLAUDE.md starts with this:"
```
🚨 CRITICAL RULE 🚨
NEVER touch the database structure without my permission.
```

"That's like telling a contractor: 'You can paint the walls, but don't knock down any walls without asking.'"

**Layer 2: Locked Rooms (what they can't touch)**
"Certain files are completely off-limits:"
- Database schema (like the building's foundation)
- Payment code (like the safe)
- Configuration files (like the alarm system)

"Even if an agent tries, the system blocks it."

**Layer 3: Security Cameras (what they did)**
"Every action is logged. I can see:"
- What files were opened
- What changes were made
- What commands were run
- Whether anything failed

"Complete audit trail. If something goes wrong, I can trace exactly what happened."

**Layer 4: The Final Gate (human approval)**
"Nothing reaches customers without me approving it. Period. This is the most important guardrail."

### The Contractor Analogy
"Think of it like hiring a general contractor:"
- **Handbook** = the project specs
- **Locked rooms** = "don't touch the electrical without a permit"
- **Security cameras** = the job site inspection log
- **Final gate** = you walk through before signing off

---

## Section 4: Real Results — Peekaboo (7 min)

### What Peekaboo Does
"Peekaboo tracks how brands appear in AI search results. When someone asks ChatGPT 'what's the best project management tool?' — does your brand show up? What position? What was cited?"

"We query 5 AI platforms: ChatGPT, Gemini, Perplexity, Google AI Mode, and Google AI Overview."

### The Numbers
- **1,162 commits** in the codebase
- **13 specialist AI agents** managing different parts
- **Automated daily analysis** runs at 2 AM
- **Paying customers** using the platform
- **Built by one person** + AI agent team

### What Would Have Been Required Traditionally
"A project this complex — 5 API integrations, automated pipelines, billing, user management, competitive analysis — would typically need 3-5 developers working 4-6 months.

I did it with AI agents at a fraction of the cost. Not because I work 80-hour weeks. Because my agents work while I sleep."

### The Product-Market Fit Story
"Here's what AI agents freed me up to do: instead of writing code all day, I spent time talking to customers, understanding their needs, and shaping the product. The agents handled implementation. I handled product-market fit.

That's the real unlock. It's not about saving money on developers. It's about spending your time on the work that actually matters — understanding your customers."

---

## Section 5: How You Can Start (5 min)

### Option 1: Direct Conversation
"Install Claude Code. Open your project. Talk to it in plain English."
```
'Look at this project and tell me what it does.'
'Add a page that shows a list of our products.'
'Write tests for the new page.'
```

"You don't need to know code to do this. You need to know what you want."

### Option 2: The Notion Board (TicketToPR)
"Have a developer set up TicketToPR. Then you:"
1. Write tickets in Notion (plain English)
2. Drag to 'Review' → AI scores feasibility
3. Drag to 'Execute' → AI builds it
4. Review the result → Approve or send back

"You work in Notion and GitHub. No terminal required."

### Option 3: Pair with a Technical Advisor
"Find a developer who understands AI agents. They set up the CLAUDE.md and guardrails. You manage the product and review process."

"This is the modern version of 'non-technical founder + technical co-founder.' Except now the 'co-founder' is an AI team managed by a part-time technical advisor."

---

## Closing (3 min) — Shared with Both Tracks

### The Free Resource
"Everything I showed you today is in a public repo:"

**Show:** `github.com/[your-username]/ai-agent-playbook`

What's inside:
- **Starter template** — copy this CLAUDE.md into any project
- **Agent specs** — real examples of specialist agents
- **Founder guide** — managing AI without coding
- **Dev guide** — engineering principles for AI agents
- **Core principles** — KISS, testing, Definition of Done

### The One Takeaway
"AI solves the *how*, not the *what*. Your job is knowing what to build. Start with a CLAUDE.md — your AI team's employee handbook. Add one rule. See what happens."

### The YC Quote
"Y Combinator said it best: 'Cursor and Claude Code are great at helping build software once it's clear what needs to be built. But writing code is only part of building a product. The most important part is figuring out what to build in the first place.'

That's your superpower. You know what to build. Now you have a team to build it."

---

## Q&A Notes

**Likely founder questions:**
- "Do I still need a CTO?" → "For architecture decisions and setting up guardrails, you need someone technical — but it could be a part-time advisor, not a full-time hire."
- "What if I don't have a project yet?" → "Start with Claude Code and a blank Next.js project. Describe what you want in plain English. You'll be surprised how far it gets."
- "Is this just for web apps?" → "Claude Code works with any codebase — web, mobile, APIs, scripts. The principles apply broadly."
- "How do I know the code is good?" → "The same way you know a contractor's work is good: tests pass, it does what you asked, and someone qualified reviewed it."
- "What about security?" → "AI-generated code needs the same security review as human-written code. The guardrails and review process exist for exactly this reason."
