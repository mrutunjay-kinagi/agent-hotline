---
name: agent-hotline
description: Structured handoffs for software development workflows (Builder → QA → DevOps).
---

# Agent Hotline: Software Team Coordination

Use this skill to coordinate Builder, QA, and DevOps agents on the same project.

Instead of Slack threads and copy/paste, all agents read/write state to `.hotline.json` for auditable, structured handoffs.

## The workflow

```
Builder writes code locally (free)
    ↓
QA reads and tests thoroughly
    ↓
DevOps reads QA approval and deploys safely
    ↓
Cycle repeats
```

## Commands

### `agent-hotline read`
Load the latest handoff from the previous agent. Run this first.

### `agent-hotline write "<status>"`
Hand off your work to the next agent. Example:
```
agent-hotline write "OAuth2 implemented. All 15 tests passing. Ready for integration testing."
```

### `agent-hotline clear`
Reset the state after deployment. Run this once the feature is live and stable.

## Roles

| Role | Task | Model | Cost |
|------|------|-------|------|
| **Builder** | Code generation, unit testing | Local (GPU/CPU) | $0 |
| **QA** | Integration testing, edge cases | Cloud | $ |
| **DevOps** | Deployment, monitoring | Cloud | $ |

## Tips

- **Builder:** Run `npm run test` locally before handing off.
- **QA:** Test aggressively. Include logs in your handoff.
- **DevOps:** Review staging metrics before going to production.
- **All:** Keep handoffs brief (2-3 sentences). Commit to git frequently.
