# 🚀 Agent-Hotline: Structured Developer Workflows

An open-source Claude Code skill for **handoff-driven software development**.

Coordinate Builder → Architect → DevOps across separate terminals through `.hotline.json` instead of Slack/email threads.

---

## 🧭 The Vision

Developers work in parallel:
- **Builder** codes locally (free iteration, GPU or CPU)
- **Architect** tests independently (catches edge cases early)
- **DevOps** deploys with confidence (auditable handoff trail)

Each agent reads the previous state, does their work, and hands off. No copy/paste. No context loss. Async but structured.

### Architecture

```
┌──────────────────────────────────────────┐
│     PROJECT WORKSPACE DIRECTORY          │
└────────────────────┬─────────────────────┘
                     │
    ┌────────────────┼────────────────┐
    ▼                ▼                ▼
┌─────────────┐ ┌─────────────┐ ┌─────────────┐
│ TERMINAL 1  │ │ TERMINAL 2  │ │ TERMINAL 3  │
│   BUILDER   │ │     Architect      │ │   DEVOPS    │
├─────────────┤ ├─────────────┤ ├─────────────┤
│ Code Gen    │ │ Test Runs   │ │ Deploy      │
│ Unit Tests  │ │ Edge Cases  │ │ Monitoring  │
│ Local Model │ │ Cloud Model │ │ Cloud Model │
│   (Free)    │ │   ($)       │ │   ($)       │
└──────┬──────┘ └──────┬──────┘ └──────┬──────┘
       │                │               │
       └────────────────┼───────────────┘
                    .hotline.json
                (Single source of truth)
```

## ✨ Why Open Source?

| Benefit | Why it matters |
|---------|---|
| **🔓 Model freedom** | Keep the workflow neutral so any local or cloud model can plug in. No vendor lock-in. |
| **🔍 Auditability** | Handoffs live in a plain local `.hotline.json` file for transparent review. |
| **🚀 Community velocity** | Share templates, prompts, and role patterns across teams. Fork and adapt freely. |

## 💡 The Problem We Solve

**Traditional AI dev has a painful tradeoff:**

| Approach | Cost | Speed | Quality |
|----------|------|-------|---------|
| **Cloud-only** (Claude, GPT, Gemini) | $$$ per token | ⚡ Fast | ✅ High |
| **Local-only** (Ollama, LM Studio) | $0 | 🐢 Slow | ⚠️ Varies |
| **agent-hotline** (Hybrid) | $ (minimal) | ⚡ Fast | ✅ High |

**The solution:** Run a free, fast local **Builder** for iteration and a cloud **Architect** for review and guidance.

---

## 🎯 Core Roles

### 🏗️ The Builder
- **Task:** Code generation, scaffolding, unit testing
- **Model:** Any local runner (Ollama, LM Studio, vLLM, llama.cpp)
- **Cost:** $0 (your hardware)
- **Speed:** ⚡ Fast (runs on GPU/CPU)

### 🧪 The Architect Engineer
- **Task:** Integration testing, edge case discovery, quality validation
- **Model:** Cloud model (Claude, GPT, Gemini)
- **Cost:** $ (pay-per-token, minimal with caching)
- **Speed:** ⚡ Fast (cloud-scaled)

### 🚀 The DevOps Engineer
- **Task:** Deployment, monitoring, infrastructure validation
- **Model:** Cloud model (Claude, GPT, Gemini)
- **Cost:** $ (pay-per-token, minimal with caching)
- **Speed:** ⚡ Fast (cloud-scaled)

---

## 💡 Why This Structure?

| Metric | Builder | Architect | DevOps |
|--------|---------|-----|--------|
| **Best for** | Heavy iteration & generation | Finding bugs, edge cases | Safe deployments |
| **Cost model** | Local (free) | Cloud (pay per test) | Cloud (pay per deploy) |
| **Independence** | Works offline | Waits for Builder | Waits for Architect approval |

---

## 🛠️ Getting Started

### Setup

1. Install the skill in **each** Claude Code terminal:
   ```bash
   /plugin install agent-hotline-skills@agent-hotline
   ```

2. All agents share the same workspace directory (same `.hotline.json`).

### Example: Feature → Architect → Deploy

**Terminal 1 (Builder, Local Model):**
```bash
# Read any pending work
agent-hotline read

# Code the feature
# → Write OAuth2 flow in src/auth/oauth.ts
# → Write unit tests in tests/auth.test.ts
# → Run: npm run test (all pass)

# Hand off to Architect
agent-hotline write "Implemented OAuth2 flow. All unit tests passing (15/15). Ready for integration testing."
```

**Terminal 2 (Architect, Cloud Model):**
```bash
# Read the handoff from Builder
agent-hotline read

# Test thoroughly
# → Run integration tests
# → Try concurrent requests
# → Test token expiration edge cases

# Report findings
agent-hotline write "Found edge case: concurrent token refresh causes race condition. Builder: fix needed before deploying."
```

**Terminal 1 (Builder, Local Model):**
```bash
# Read the Architect report
agent-hotline read

# Fix the issue
# → Add mutex lock to token refresh
# → Run tests again (all pass)

# Hand off to DevOps
agent-hotline write "Fixed race condition with mutex. All tests passing (18/18). Ready to deploy to staging."
```

**Terminal 3 (DevOps, Cloud Model):**
```bash
# Read the handoff from Builder
agent-hotline read

# Deploy and monitor
# → Deploy to staging environment
# → Run smoke tests
# → Monitor metrics (latency, errors)

# Final approval
agent-hotline write "Deployed to staging. All metrics green. Ready for production rollout."
agent-hotline clear
```

---

## 🌉 The Handoff Protocol

### The Bridge: `agent-hotline` CLI

Store and sync state through a local file instead of copy/paste:

```bash
# Architect writes a task for Builder
agent-hotline write "Plan: Implement OAuth2 flow in src/auth/. Builder: run tests after."

# Builder reads and executes
agent-hotline read
# → [output of latest handoff]

# Builder updates progress
agent-hotline write "Implemented OAuth2. All tests passing. Ready for security review."

# DevOps reviews and clears
agent-hotline clear
```

**What gets stored:** `.hotline.json` (plaintext, local, auditable)

Schema:

```json
{
  "schemaVersion": 1,
  "current": {
    "type": "handoff",
    "id": "1748490000000-ab12cd",
    "message": "Implemented OAuth2. All tests passing. Ready for security review.",
    "author": "alice",
    "createdAt": "2026-05-29T10:00:00.000Z"
  },
  "history": [
    {
      "type": "handoff",
      "id": "1748490000000-ab12cd",
      "message": "Implemented OAuth2. All tests passing. Ready for security review.",
      "author": "alice",
      "createdAt": "2026-05-29T10:00:00.000Z"
    },
    {
      "type": "clear",
      "author": "bob",
      "createdAt": "2026-05-29T11:00:00.000Z"
    }
  ],
  "lastClearedAt": "2026-05-29T11:00:00.000Z"
}
```

The `author` field captures the system user or custom role name. Set the role explicitly via:

```bash
export HOTLINE_AUTHOR=builder
agent-hotline write "..."

export HOTLINE_AUTHOR=architect
agent-hotline write "..."
```

Real handoff loop:
1. `agent-hotline write "<status>"`
2. `agent-hotline read`
3. `agent-hotline status`
4. `agent-hotline clear` after stable deployment

---

## 🔄 The Ideal Workflow

```
[Builder Codes] ──► [Architect Tests] ──► [DevOps Deploys] ──► [Repeat]
```

### Step-by-step

1. **🎯 Scope the feature**
   - PM/Lead writes scope in issue tracker or wiki.
   - Builder reads scope.

2. **🏗️ Builder implements locally**
   ```bash
   agent-hotline read
   # → Read the feature scope
   
   # Write code locally (free iteration)
   npm run test  # All passing
   
   agent-hotline write "OAuth2 flow implemented. All unit tests passing (15/15). Ready for integration testing."
   ```

3. **🧪 Architect validates thoroughly**
   ```bash
   agent-hotline read
   # → Read Builder's handoff
   
   # Run integration tests, edge cases, load tests
   npm run test:integration
   
   agent-hotline write "Integration tests: 18/18 passing. Found 1 edge case: concurrent token refresh. Builder: needs fix."
   ```

4. **🔧 Builder fixes issues**
   ```bash
   agent-hotline read
   # → Read Architect's findings
   
   # Fix and re-test
   npm run test  # All passing
   
   agent-hotline write "Fixed race condition. All tests passing (21/21). Ready for deployment."
   ```

5. **🚀 DevOps deploys with confidence**
   ```bash
   agent-hotline read
   # → Read Architect approval + Builder confirmation
   
   # Deploy to staging, then production
   npm run deploy:staging
   npm run deploy:prod
   
   agent-hotline write "Deployed to production. Monitoring: all metrics green. Rollout complete."
   agent-hotline clear
   # → Reset state for next feature
   ```

---

## 📊 Cost & Performance

| Metric | Builder | Architect | DevOps |
|--------|---------|-----|--------|
| **💰 Cost** | $0 (local) | $ (cloud) | $ (cloud) |
| **⚡ Speed** | Fast (GPU) | Fast (cloud) | Fast (cloud) |
| **📦 Model** | Any local runner | Any cloud model | Any cloud model |
| **🔒 Privacy** | Stays offline | Cloud-secure | Cloud-secure |
| **Best for** | Iteration loops | Comprehensive testing | Deployment safety |

**Cost savings example:**
- Builder generates code locally (free) → Architect runs 50 test iterations (cloud, $1-5)
- Traditional: 50 iterations on cloud = $50-100
- agent-hotline: 49 iterations local + 1 final cloud test = $1-5 ✅ |

---

## 🎓 Best Practices

### For the Builder 🏗️
- Write scoped, modular code (easier to review)
- Run unit tests locally before handing off
- Keep commits atomic and descriptive
- Flag any limitations or assumptions in the handoff

### For the Architect Engineer 🧪
- Test aggressively: edge cases, concurrency, error handling
- Include test logs and reproduction steps in the handoff
- Don't approve until you're confident
- Ask Builder for clarification if something's unclear

### For the DevOps Engineer 🚀
- Review logs and metrics during staging deployment
- Have a rollback plan before prod deployment
- Monitor in real time after going live
- Clear the state only after confirming stability

### For all roles 👥
- Keep handoffs brief (status + blockers, 2-3 sentences)
- Commit frequently to git so changes are auditable
- Use meaningful branch names (e.g., `feature/oauth2`)
- Close the `.hotline.json` loop: finish what you start

---

## 🚀 Next Steps

- Install the Claude Code skill: See [README](../README.md)
- Set up your local model runner (Ollama, LM Studio, etc.)
- Set up your cloud endpoint (Claude Code + Bedrock, etc.)
- Run your first handoff cycle!

Questions? Open an issue or join the community.
