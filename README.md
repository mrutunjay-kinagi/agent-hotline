# agent-hotline

Open-source Claude Code skill + CLI for **structured handoffs** in software development workflows.

Pair specialized agents (Builder, Architect, DevOps) on separate terminals. The Builder can iterate locally, while Architect/DevOps verify in cloud terminals using a shared `.hotline.json` state file.

## Why

- **Builder** generates code locally (free iteration)
- **Architect** tests independently (catches issues early)
- **DevOps** deploys with confidence (auditable handoff trail)

No context loss. No copy/paste. Async coordination that scales.

## Install

### CLI (runnable today)

```bash
npm install -g git+https://github.com/mrutunjay-kinagi/agent-hotline.git
```

Then run:
```bash
agent-hotline help
```

### Claude Code skill (optional, for plugin workflow)

1. Add this repo to your Claude Code plugin marketplace:
   ```
   /plugin marketplace add mrutunjay-kinagi/agent-hotline
   ```
2. Install the skill in **each** agent terminal:
   ```
   /plugin install agent-hotline-skills@agent-hotline
   ```

## Quick start: Feature → Architect → Deploy

**Terminal 1 (Builder):**
```bash
agent-hotline write "Feature: OAuth2 flow in src/auth/. Tests needed in tests/auth.test.ts. Ready: ✓"
```

**Terminal 2 (Architect):**
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
- `agent-hotline status` — Show active handoff + history count
- `agent-hotline clear` — Cycle complete, ready for next feature

All state stored in `.hotline.json` (plaintext, git-tracked, auditable).

## `.hotline.json` schema

```json
{
  "schemaVersion": 1,
  "current": {
    "type": "handoff",
    "id": "1748490000000-ab12cd",
    "message": "OAuth2 implemented. 15/15 unit tests passing. Ready for Architect.",
    "author": "mj",
    "createdAt": "2026-05-29T10:00:00.000Z"
  },
  "history": [
    {
      "type": "handoff",
      "id": "1748490000000-ab12cd",
      "message": "OAuth2 implemented. 15/15 unit tests passing. Ready for Architect.",
      "author": "mj",
      "createdAt": "2026-05-29T10:00:00.000Z"
    },
    {
      "type": "clear",
      "author": "sarah",
      "createdAt": "2026-05-29T12:00:00.000Z"
    }
  ],
  "lastClearedAt": "2026-05-29T12:00:00.000Z"
}
```

The `author` field defaults to your system username. To set a custom author (e.g., role name):

```bash
export HOTLINE_AUTHOR=builder
agent-hotline write "Feature implemented. Ready for Architect."

export HOTLINE_AUTHOR=architect
agent-hotline read
agent-hotline write "Tests: 11/12 passing. One edge case found."
```

Typical cycle:
1. Builder runs `agent-hotline write "<status>"`
2. Architect/DevOps run `agent-hotline read`, then write updates
3. DevOps runs `agent-hotline clear` after rollout is stable

## Learn more

- **[FRAMEWORK.md](./docs/FRAMEWORK.md)** — workflows, architecture, best practices
- **[Copilot instructions](./.github/copilot-instructions.md)** — for contributors

## Community

- Open an issue with workflow ideas or improvements
- PR welcome for features, docs, examples
- Fork and customize for your team's process
