# Copilot instructions

## Project overview
- This repository documents the `agent-hotline` workflow: an open-source Claude Code skill for structured handoffs in software development.
- Multiple terminals coordinate: Builder (local model), Architect (cloud), DevOps (cloud) all working on the same codebase via `.hotline.json`.

## High-level architecture
- Coordination is done through the `agent-hotline` CLI, which writes/reads shared state in `.hotline.json` using `write`, `read`, and `clear` commands.
- The intended workflow is: Builder codes locally → `agent-hotline write` handoff → Architect reviews thoroughly → DevOps deploys safely.
- The repo ships as a Claude Code skill/plugin so all agents use the same handoff protocol.

## Key conventions
- Use `agent-hotline write/read` for cross-agent handoffs instead of Slack/email.
- Keep the Builder → Architect → DevOps role sequence intact when proposing workflow changes.
- Builder should always run tests locally before handing off; Architect should verify thoroughly before DevOps deploys.
