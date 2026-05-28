# 🚀 Hybrid Dual-Agent Development Framework

An open-source, model-agnostic workflow that turns two Claude Code terminals into a **Builder + Architect** team.

Use any local model for the Builder (GPU, CPU, or self-hosted) and any cloud model for the Architect, with `agent-hotline` keeping the handoff state in sync.

---

## 🧭 The Vision

Build locally with any LLM runner (Ollama, LM Studio, vLLM, llama.cpp, or a custom server) while a cloud model reviews and guides—`agent-hotline` keeps the handoff state synced without copy/paste.

### Architecture

```
┌──────────────────────────────────────────┐
│     PROJECT WORKSPACE DIRECTORY          │
└────────────────────┬─────────────────────┘
                     │
         ┌───────────┴───────────┐
         ▼                       ▼
    ╔════════════════╗   ╔═══════════════════╗
    ║  TERMINAL 1    ║   ║   TERMINAL 2      ║
    ║  LOCAL BUILDER ║◄──┤ CLOUD ARCHITECT   │
    ╠════════════════╣   ╠═══════════════════╣
    ║ Any Model      ║   ║ Any Model         ║
    ║ GPU/CPU/Remote ║   ║ Claude/Gemini/etc ║
    ║ (Free)         ║   ║ (Pay-per-token)   ║
    ║                ║   ║                   ║
    ║ Code Gen       ║───│ Plan, Review      ║
    ║ Testing        │◄──│ Audit, Unblock    ║
    ║ Heavy Edits    ║   │                   ║
    ╚════════════════╝   ╚═══════════════════╝
         ▲                       ▲
         │   agent-hotline        │
         │   (Shared State)      │
         └───────────┬───────────┘
                     │
            .hotline.json
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

### 🏗️ The Builder (Local Model)
- **Cost:** $0.00 (your hardware)
- **Speed:** ⚡ Fast (GPU/CPU at hand)
- **Specialization:** Code generation, scaffolding, heavy edits, testing loops
- **Model:** Any local runner (Ollama, LM Studio, vLLM, llama.cpp)

### 🧠 The Architect (Cloud Model)
- **Cost:** $ (pay-per-token, minimal with caching)
- **Quality:** ✅ High-performance reasoning
- **Specialization:** Strategic planning, security review, architecture validation, unblocking
- **Model:** Any cloud provider (Claude, Gemini, etc.)

---

## 🛠️ Getting Started

### 🎛️ Terminal 1: Local Builder

**Example: Ollama with a high-context coding model**

```bash
# Create a custom Modelfile with 64k context
echo -e "FROM qwen3.5\nPARAMETER num_ctx 65536" > Modelfile
ollama create claude-qwen -f ./Modelfile

# Launch
ollama launch claude --model claude-qwen
```

> Replace Ollama with any local runner you prefer: LM Studio, vLLM, llama.cpp, text-generation-webui, etc.

### ☁️ Terminal 2: Cloud Architect

**Example: Claude via AWS Bedrock**

```bash
# Set up credentials
export CLAUDE_CODE_USE_BEDROCK=1
export AWS_REGION=us-east-1

# Launch Claude Code with Bedrock backend
claude
```

> Works with any cloud provider: OpenAI, Anthropic, Google, Mistral, etc.

---

## 🌉 The Handoff Protocol

### The Bridge: `agent-hotline` CLI

Store and sync state through a local file instead of copy/paste:

```bash
# Architect writes a task
agent-hotline write "Plan: Implement OAuth2 flow in src/auth/. Builder: run tests after."

# Builder reads and executes
agent-hotline read
# → [output of latest handoff]

# Builder updates progress
agent-hotline write "Implemented OAuth2. All tests passing. Ready for security review."

# Architect reviews and clears
agent-hotline clear
```

**What gets stored:** `.hotline.json` (plaintext, local, auditable)

---

## 🔄 The Ideal Workflow

```
[Architect Plans] ──► [Builder Codes] ──► [Architect Reviews] ──► [Repeat]
```

### Step-by-step

1. **📋 Architect writes a scoped plan**
   ```
   Ask Architect to break down the issue into concrete steps with success criteria.
   ```

2. **✍️ Architect hands off via agent-hotline**
   ```bash
   agent-hotline write "Step 1: Implement JWT validation in src/auth/token.ts. 
   Tests in tests/auth.test.ts. Success: all tests pass."
   ```

3. **🏗️ Builder reads and starts building**
   ```bash
   agent-hotline read
   # → [reads the plan]
   # → Starts implementing locally (free iteration)
   ```

4. **🧪 Builder iterates locally**
   ```bash
   # Run tests, compile, refactor—all free compute cycles
   npm run test
   npm run build
   ```

5. **📤 Builder hands off results**
   ```bash
   agent-hotline write "Implemented JWT validation. All 12 tests passing. 
   Ready for security review."
   ```

6. **🔍 Architect reviews and validates**
   ```
   Read the code, check for security issues, edge cases, architecture fit.
   ```

7. **✅ Architect clears and approves**
   ```bash
   agent-hotline clear
   # → Cycle complete. Ready for next task.
   ```

---

## 📊 Cost & Performance

| Metric | Local Builder | Cloud Architect |
|--------|---|---|
| **💰 Cost** | $0 | $$ (pay-per-token) |
| **⚡ Speed** | Fast (local) | Fast (cloud) |
| **🔒 Privacy** | Offline, stays on machine | Secure cloud compliance ring |
| **🎯 Best for** | Generation, tests, iteration | Planning, review, strategy |
| **📦 Model** | Any local runner | Any cloud provider |
| **🖥️ Compute** | Your hardware | Cloud-scaled |

---

## 🎓 Best Practices

### For the Architect 🧠
- Write clear, scoped specs with success criteria (not vague goals).
- Include examples of expected output.
- Flag security/performance concerns early.
- Review code for architectural fit, not style.

### For the Builder 🏗️
- Read the handoff carefully before starting.
- Run tests locally before passing back.
- Keep commits focused and atomic.
- Ask for clarification if the spec is unclear.

### For both 👥
- Keep handoffs brief (2-3 sentences of status).
- Use `agent-hotline clear` to reset between major work streams.
- Commit to git regularly so changes are visible to both agents.

---

## 🚀 Next Steps

- Install the Claude Code skill: See [README](../README.md)
- Set up your local model runner (Ollama, LM Studio, etc.)
- Set up your cloud endpoint (Claude Code + Bedrock, etc.)
- Run your first handoff cycle!

Questions? Open an issue or join the community.
