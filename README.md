# agent-hotline

Open-source Claude Code skill for **structured handoffs** in software development workflows.

Pair specialized agents (Builder, QA, DevOps) on separate terminals. They collaborate through `.hotline.json` instead of Slack/email chaos.

## Why

- **Builder** generates code locally (free iteration)
- **QA** tests independently (catches issues early)
- **DevOps** deploys with confidence (auditable handoff trail)

No context loss. No copy/paste. Async coordination that scales.

## Install

1. Add this repo to your Claude Code plugin marketplace:
   ```
   /plugin marketplace add mrutunjay-kinagi/agent-hotline
   ```
2. Install the skill in **each** agent terminal:
   ```
   /plugin install agent-hotline-skills@agent-hotline
   ```

## Quick start: Feature → QA → Deploy

**Terminal 1 (Builder):**
```bash
agent-hotline write "Feature: OAuth2 flow in src/auth/. Tests needed in tests/auth.test.ts. Ready: ✓"
```

**Terminal 2 (QA):**
```bash
agent-hotline read
# → Runs tests, finds edge case
agent-hotline write "Tests: 11/12 passing. Edge case: concurrent token refresh. Needs fix."
```

**Terminal 1 (Builder):**
```bash
agent-hotline read
# → Fixes concurrent issue
agent-hotline write "Fixed with mutex lock. All tests passing: 12/12. Ready for deployment."
```

**Terminal 3 (DevOps):**
```bash
agent-hotline read
# → Deploys to staging
agent-hotline write "Deployed to staging. Monitoring metrics. All green. Ready for prod."
agent-hotline clear
```

## How it works

- `agent-hotline write` — Senior/lead agent hands off work
- `agent-hotline read` — Next agent in chain reads context
- `agent-hotline clear` — Cycle complete, ready for next feature

All state stored in `.hotline.json` (plaintext, git-tracked, auditable).

## Learn more

- **[FRAMEWORK.md](./docs/FRAMEWORK.md)** — workflows, architecture, best practices
- **[Copilot instructions](./.github/copilot-instructions.md)** — for contributors

## Community

- Open an issue with workflow ideas or improvements
- PR welcome for features, docs, examples
- Fork and customize for your team's process
