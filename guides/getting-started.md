# Getting Started: Your First 30 Minutes

## Prerequisites

- A computer with a terminal (Mac, Linux, or WSL on Windows)
- Node.js 18+ installed ([download](https://nodejs.org))
- A Claude account (you'll need a Claude Pro or Teams subscription)

## Step 1: Install Claude Code (2 minutes)

```bash
npm install -g @anthropic-ai/claude-code
```

Verify it's installed:
```bash
claude --version
```

## Step 2: Authenticate (1 minute)

```bash
claude auth
```

Follow the prompts to sign in with your Claude account.

## Step 3: Create Your First CLAUDE.md (10 minutes)

Navigate to your project:
```bash
cd ~/your-project
```

Copy the template from this playbook:
```bash
cp ~/Projects/ai-agent-playbook/templates/CLAUDE.md.template ./CLAUDE.md
```

Open it in your editor and customize:
1. **Critical Rules** — What should the agent NEVER do? (database migrations, deleting files, etc.)
2. **Project Overview** — One paragraph about what this project is
3. **Commands** — Your build, test, and lint commands
4. **Definition of Done** — What "done" looks like

**Tip:** Start small. 20 lines is fine. You'll add more rules as you discover what agents get wrong.

## Step 4: Start a Conversation (5 minutes)

```bash
claude
```

You're now talking to Claude Code, and it has read your CLAUDE.md. Try:

```
> Can you read my CLAUDE.md and summarize what you know about this project?
```

It should recite your rules back to you. If it can, the constitution is working.

## Step 5: Your First Task (10 minutes)

Start with something small and verifiable:

```
> Add a health check endpoint at /api/health that returns { status: "ok", timestamp: <current time> }. Write a test for it.
```

Watch what happens:
1. Claude reads your codebase to understand patterns
2. Claude creates the endpoint following your conventions
3. Claude writes a test
4. Claude runs the test

**Review the output.** Does it match your project's patterns? Did it follow your CLAUDE.md rules?

## Step 6: The Feedback Loop

If Claude did something wrong:
1. Tell it what to fix: `"The test should check the response status code too"`
2. **Add a rule to your CLAUDE.md** so it doesn't happen again:
   ```
   - All API endpoint tests must verify the HTTP status code
   ```

This is how you train your agent. Every mistake becomes a permanent rule.

## What's Next

- **Dev Track:** Read [dev-track.md](dev-track.md) for SOLID principles, agent-reviews-agent patterns, and advanced configuration
- **Founder Track:** Read [founder-track.md](founder-track.md) for the mental model of managing an AI team
- **Agent Specs:** Look at the [examples/](../examples/) folder for real agent definitions you can adapt
- **Orchestration:** Check out [TicketToPR](https://www.npmjs.com/package/ticket-to-pr) to automate the full ticket-to-PR workflow

## Common First-Session Questions

**Q: How much does this cost?**
A: Claude Code charges based on token usage. A simple task (add an endpoint, write a test) typically costs $0.10-$0.50. A complex feature might cost $2-5. You always see the cost after each session.

**Q: Can it access the internet?**
A: By default, no. You can whitelist specific domains in your Claude Code settings if needed.

**Q: What if it makes a mistake?**
A: That's expected. Review everything. Use `git diff` to see what changed. Reject what doesn't look right. Add a rule to prevent the mistake next time.

**Q: Is my code sent to Anthropic?**
A: Yes, Claude Code sends your code to Anthropic's API for processing. It's covered by Anthropic's usage policy. Don't use it on code you can't share with a cloud provider.
