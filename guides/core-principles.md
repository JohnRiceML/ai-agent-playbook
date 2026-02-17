# Core Principles: Engineering with AI Agents

These principles apply whether you code or not. They're the foundation of working effectively with AI agents.

---

## 1. KISS — Keep It Simple, Stupid

**The principle:** One agent, one job. Don't build a "do everything" agent.

**Why it matters for AI:** A general-purpose agent will produce mediocre results across every domain. A specialist agent with narrow focus and deep context produces expert-level results.

**In practice:**
- Bad: "Review my code, write tests, check the pipeline, and update the docs"
- Good: "You are the Scoring Auditor. Your only job is verifying that the scoring formula is consistent everywhere."

**The team analogy:** You wouldn't hire one person to be your accountant, lawyer, and designer. Same with agents.

---

## 2. SOLID — Applied to AI Agents

These are software engineering principles that map perfectly to agent design:

### Single Responsibility
Each agent owns **one domain**. The scoring auditor doesn't review UI. The pipeline medic doesn't refactor business logic.

### Open/Closed
Agent specs are open for extension (you can add rules over time) but closed for modification (the core role doesn't change). When the scoring auditor finds a new failure mode, you add a check to its spec — you don't change what it is.

### Liskov Substitution
Any agent following the spec template produces predictable, structured output. You know exactly what format the report will be in. Swap in a different model and the output contract stays the same.

### Interface Segregation
Give agents only the tools they need:
- **Read-only agents** (auditors, reviewers): Read, Glob, Grep
- **Write agents** (implementers): Read, Edit, Write, limited Bash
- **No agent** gets full system access by default

### Dependency Inversion
Agents depend on your **CLAUDE.md** (the project constitution), not on each other. The scoring auditor reads the CLAUDE.md to find the scoring formula source of truth. It doesn't call the pipeline medic for information.

---

## 3. Testing, Testing, Testing

**The principle:** Don't assume AI-generated code works. Write a simple test for anything you can. Build from there.

### Why this matters — the data

| Stat | Source |
|------|--------|
| AI code has **1.7x more issues** than human code | CodeRabbit, Dec 2025 |
| **2.74x more XSS vulnerabilities** | CodeRabbit, Dec 2025 |
| Only **55%** of AI-generated code is secure | Veracode, 2025 |
| **37.6% more critical vulns** after 5 iterations of AI refinement | IEEE-ISTAS, 2025 |
| Performance issues appear **~8x more** in AI code | CodeRabbit, Dec 2025 |
| **94%** of LLM compilation errors are type-check failures | Academic study, 2025 |
| **8,000+ startups** need rebuilds after "vibe coding" | TechStartups, Dec 2025 |

This isn't because AI is bad — it's because AI is **confident**. It doesn't say "I'm not sure about this edge case." It writes the code and moves on. Without tests and review, those confident mistakes ship to production.

**The experience gap (Qodo, 2025):** Senior developers (10+ years) report the highest quality improvements from AI (68%) but the most caution — only 26% would ship unreviewed. Junior developers report the lowest quality improvements (52%) yet the highest confidence shipping without review (60%). **The least experienced are the most trusting.**

### The testing workflow

```
1. Agent writes code
2. Agent writes a simple test for the happy path
3. Agent runs the test
4. You review: Does the test actually test what matters?
5. You ask: "What edge cases are missing?"
6. Agent adds edge case tests
7. All tests pass → code is reviewable
```

### The agent-reviews-agent pattern
The most powerful pattern: have one agent build, and a second agent review.

```
Execute Agent → writes code and tests
                    ↓
Scoring Auditor → verifies formula consistency
Code Reviewer   → checks for N+1 queries, auth holes
Pipeline Medic  → validates timeout configurations
```

This is not redundant — it's the same reason engineering teams have code review. Fresh eyes (even artificial ones) catch what the author missed.

### Watch out: "Tests as Ceremony"

Mark Seemann (2026) warns that AI-generated tests are often **tautological** — the AI generates code, then generates tests that pass for that code. You never see them fail, so you never know if they test anything.

**How to avoid it:**
- Specify test **intent** explicitly: "Verify a user cannot access another user's data even with a valid session token"
- Demand **edge cases** by name: null input, empty string, max integer, concurrent access
- Require tests **before** implementation — if the AI hasn't seen the code, it can't write tautological tests
- Review for test **value**, not test count — one meaningful test beats ten tautological ones

### What to test
- **Always test:** Business logic, API contracts, data transformations
- **Usually test:** Component rendering, error handling
- **Sometimes test:** Styling, layout (snapshot tests)
- **Never skip:** Auth, payment, data integrity

### Real-world cautionary tales

- **Moltbook (Jan 2026):** AI agents built the backend. At no point did they enable Row Level Security. Leaked **1.5 million API keys** within 3 days.
- **Replit incident (2025):** An AI tool deleted a **live company database during a code freeze**. 1,200 executives' data wiped.
- **The vibe coding hangover:** ~10,000 startups built with AI assistants; 8,000+ now need rebuilds at $50K-$500K each.

---

## 4. Definition of Done

AI code is "done" when ALL of these pass:

| Check | Command | Why |
|-------|---------|-----|
| Types | `tsc --noEmit` | Catches type mismatches before runtime |
| Tests | `npm test` / `vitest run` | Verifies behavior matches intent |
| Lint | `npm run lint` | Catches style issues and common bugs |
| Build | `npm run build` | Proves it works in production mode |
| Review | Human reads the diff | You can explain *why* the code works |
| Fit | Check against architecture | No new patterns that contradict existing ones |

**The key question:** Can you explain why this code works, not just that it works?

If you can't explain it, it's not done. Ask the agent to explain its approach. If the explanation doesn't make sense, the code probably has a subtle bug.

---

## 5. Ground Everything in Real Data

**The principle:** Don't let agents guess. Give them numbers, formulas, file paths, and examples.

### Bad (vague instruction):
```
Set reasonable timeouts for the API endpoints.
```

### Good (grounded in data):
```
Set timeouts using P90 latency + 20% buffer:
- ChatGPT: P90 is 58s → soft timeout 70s
- Gemini: P90 is 29s → soft timeout 35s
- Perplexity: P90 is 20s → soft timeout 24s
Never set timeouts based on averages.
```

### What to ground:
- **Formulas:** Write out the math. Include a worked example.
- **Benchmarks:** Include actual latency, throughput, or cost numbers.
- **File paths:** Tell the agent exactly where the source of truth lives.
- **Examples:** Show what correct output looks like, verbatim.
- **Verification logs:** Show what the agent should see if things are working.

### Why this works
Agents are excellent at following precise specifications. They're mediocre at making judgment calls. The more concrete data you provide, the better the result.

---

## 6. Review and Verify — Trust but Confirm

**The principle:** AI agents are your team, not your replacement. Review their output like you'd review a junior dev's PR.

### The review checklist:
1. **Does it do what was asked?** (Not more, not less)
2. **Does it follow existing patterns?** (Architecture fit)
3. **Are the tests meaningful?** (Not just passing — actually testing the right thing)
4. **Are there unexplained additions?** (New dependencies, new patterns)
5. **Can you explain the approach?** (If not, dig deeper)

### The feedback loop:
When an agent makes a mistake, don't just fix it. **Update the agent's spec or CLAUDE.md** so it never makes that mistake again. This is how you "train" your team over time.

```
Agent makes mistake → You catch it in review → You add a rule to CLAUDE.md → Agent never repeats it
```

This is the equivalent of a post-mortem. The difference is that an AI agent actually reads and follows the post-mortem document every time.
