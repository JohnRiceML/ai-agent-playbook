# Resources & References

Links and research cited in the talk and guides. Updated with latest 2025-2026 findings.

---

## Hard Data — Stats to Cite

| Stat | Source | Credibility |
|------|--------|-------------|
| **45%** of AI code introduces OWASP Top 10 vulnerabilities | [Veracode GenAI Code Security Report](https://www.veracode.com/blog/genai-code-security-report/), July 2025 | Tier 1 — established AppSec vendor, 100+ LLMs tested |
| Devs with AI wrote secure code **3%** of the time vs **21%** without | [Stanford / Dan Boneh et al.](https://arxiv.org/abs/2211.03622), ACM CCS | Tier 1 — top academic, peer-reviewed |
| AI adoption = persistent **+30% static warnings, +41% complexity** | [Carnegie Mellon](https://arxiv.org/abs/2511.04427), Nov 2025 | Tier 1 — academic, 807 repos, diff-in-diff |
| Experienced devs **19% slower** with AI (predicted 24% faster) | [METR Randomized Trial](https://arxiv.org/abs/2507.09089), July 2025 | Tier 1 — RCT, gold-standard methodology |
| **80%** of devs bypass security policies; only **10%** scan AI code | [Snyk Secure Adoption Report](https://snyk.io/reports/secure-adoption-in-the-genai-era/), 2025 | Tier 2 — major security vendor |
| **81%** of orgs knowingly ship vulnerable code to meet deadlines | [Checkmarx Future of AppSec](https://checkmarx.com/press-releases/), Aug 2025 | Tier 2 — 1,500 respondents |
| AI adoption correlated with **7.2% decrease in delivery stability** | [Google DORA Report](https://cloud.google.com/blog/products/devops-sre/announcing-the-2024-dora-report), 2024-2025 | Tier 1 — Google, industry standard |
| Trust in AI accuracy dropped from **43% to 33%** YoY | [Stack Overflow Developer Survey](https://survey.stackoverflow.co/2025/ai/), 2025 | Tier 1 — 49,000+ respondents |
| **8,000+ startups** need AI code rebuilds ($50K-$500K each) | TechStartups, 2025 | Tier 3 — trade publication |
| Devs integrate AI into **60%** of work | Anthropic, 2026 | Tier 2 — vendor self-report |
| Market projection: **$7.84B → $52.62B** by 2030 | Anthropic, 2026 | Tier 2 — vendor self-report |
| TypeScript overtook Python on GitHub (**+66.63% YoY**) | GitHub Octoverse, 2025 | Tier 1 — GitHub/Microsoft |

## Cautionary Tales (for the "why guardrails matter" section)

- **Moltbook (Jan 2026):** AI built the backend, never enabled Row Level Security. Leaked **1.5M API keys** in 3 days.
- **Replit (2025):** AI tool deleted a live database during a code freeze. 1,200 executives' data wiped.
- **The Vibe Coding Delusion:** ~10,000 startups built with AI; 8,000+ now need rescue engineering.
- **IEEE Security Paradox:** For every 10% increase in code complexity, 14.3% increase in vulnerability count.

---

## Industry Reports & Data

- [Anthropic: 2026 Agentic Coding Trends Report (PDF)](https://resources.anthropic.com/hubfs/2026%20Agentic%20Coding%20Trends%20Report.pdf?hsLang=en) — Developers integrate AI into 60% of work; market projection $7.84B (2025) → $52.62B by 2030
- [Qodo: State of AI Code Quality 2025](https://www.qodo.ai/reports/state-of-ai-code-quality/) — "Code quality is a workflow, not a checkpoint"
- [The New Stack: Vibe Coding Could Cause Catastrophic Explosions in 2026](https://thenewstack.io/vibe-coding-could-cause-catastrophic-explosions-in-2026/) — AI co-authored code: 1.7x more major issues, 2.74x more security vulnerabilities
- [MIT Technology Review: AI Coding is Now Everywhere](https://www.technologyreview.com/2025/12/15/1128352/rise-of-ai-coding-developers-2026/) — 71% of developers do not merge AI-generated code without manual review

## Claude Code & Agent Architecture

- [Anthropic: Effective Harnesses for Long-Running Agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents) — Two-agent architecture: initializer + coding agent with progress artifacts
- [Anthropic: Building Agents with the Claude Agent SDK](https://www.anthropic.com/engineering/building-agents-with-the-claude-agent-sdk) — Official SDK guide
- [Anthropic: Common Claude Code Workflows](https://code.claude.com/docs/en/common-workflows) — Core feedback loop: gather context, take action, verify
- [Anthropic: Equipping Agents with Agent Skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills) — Skills architecture

## CLAUDE.md & Guardrails

- [Matthew Groff: Implementing CLAUDE.md and Agent Skills](https://www.groff.dev/blog/implementing-claude-md-agent-skills) — 3-tier documentation: CLAUDE.md → skills → agent guides
- [Trail of Bits: claude-code-config](https://github.com/trailofbits/claude-code-config) — Opinionated CLAUDE.md defaults from a security firm
- [Codacy: Deterministic Security Guardrails in 5 Minutes](https://blog.codacy.com/equipping-claude-code-with-deterministic-security-guardrails) — "Hooks fire every time; instructions can be forgotten"
- [Shrivu Shankar: How I Use Every Claude Code Feature](https://blog.sshh.io/p/how-i-use-every-claude-code-feature) — Practitioner's complete guide (endorsed by Simon Willison)

## Engineering Principles for AI Agents

- [Qodo: The Multi-Agent Revolution](https://www.qodo.ai/blog/the-multi-agent-revolution-why-software-engineering-principles-must-govern-ai-systems/) — "Separation of Cognitive Concerns" = new Single Responsibility; 40% quality improvement with multi-agent
- [Neon: Six Principles for Production AI Agents](https://neon.com/blog/six-principles-for-production-ai-agents) — "The core feature of an AI agent is tool calling"
- [Syncfusion: SOLID Principles in AI Development](https://www.syncfusion.com/blogs/post/solid-principles-ai-development) — Encoding SOLID into agent architecture

## Competing Tools (for context)

- [Claude Code vs Cursor vs Windsurf: 2026 Guide](https://ybuild.ai/en/blog/cursor-vs-claude-code-vs-windsurf-ai-coding-tools-2026) — "Cursor = AI in your editor. Claude Code = AI in your terminal. Windsurf = AI drives."
- [Qodo: Claude Code vs Cursor](https://www.qodo.ai/blog/claude-code-vs-cursor/) — Claude Code: 80.9% on SWE-bench
- [awesome-claude-code (GitHub)](https://github.com/hesreallyhim/awesome-claude-code) — Curated skills, hooks, and plugins

## Solo Dev + AI

- [Vooster: Build Profitable Software Solo](https://www.vooster.ai/en/blog/build-profitable-software-solo) — "A single person can build, launch, and scale a profitable SaaS product"
- [Y Combinator Spring 2026 RFS](https://superframeworks.com/articles/yc-rfs-startup-ideas-indie-hackers-2026) — "The most important part is figuring out what to build"

## Vibe Coding Counter-Narrative

- [Vibing with AI: Vibe Coding Needs Guardrails](https://vibingwithai.substack.com/p/vibe-coding-needs-guardrails-what) — "The vibe coding hangover: three months later, nobody can explain how the code works"
- [Softr: 8 Vibe Coding Best Practices](https://www.softr.io/blog/vibe-coding-best-practices) — "AI tools perform dramatically better with TypeScript strict mode, ESLint, and Prettier"

## Tools Mentioned

- [TicketToPR](https://www.npmjs.com/package/ticket-to-pr) — Notion-to-PR automation with AI agents (open source)
- [Claude Code](https://claude.com/claude-code) — Anthropic's CLI for AI-powered development
- [Claude Agent SDK](https://www.npmjs.com/package/@anthropic-ai/claude-agent-sdk) — SDK for building custom agent workflows
