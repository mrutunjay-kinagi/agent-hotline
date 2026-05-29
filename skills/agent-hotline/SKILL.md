---
name: agent-hotline
description: Structured handoffs for software development workflows (local Builder → cloud Architect/DevOps).
---

# Agent Hotline: Software Team Coordination

Use this skill to coordinate a local Builder with cloud Architect and DevOps agents on the same project.

Instead of Slack threads and copy/paste, all agents read/write state to `.hotline.json` for auditable, structured handoffs.

## The workflow

```
Builder writes code locally (fast, low cost)
    ↓
Architect (cloud) reads and tests thoroughly
    ↓
DevOps (cloud) reads Architect approval and deploys safely
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

### `agent-hotline status`
Show whether there is an active handoff plus history size.

## Roles

| Role | Task | Model | Cost |
|------|------|-------|------|
| **Builder** | Code generation, unit testing | Local (GPU/CPU) | $0 |
| **Architect** | Integration testing, edge cases | Cloud | $ |
| **DevOps** | Deployment, monitoring | Cloud | $ |

## Tips

- **Builder:** Run `npm run test` locally before handing off.
- **Architect:** Test aggressively. Include logs in your handoff.
- **DevOps:** Review staging metrics before going to production.
- **All:** Keep handoffs brief (2-3 sentences). Commit to git frequently.
- **Custom authors:** Set `export HOTLINE_AUTHOR=builder` before commands to tag handoffs with role names instead of usernames.
