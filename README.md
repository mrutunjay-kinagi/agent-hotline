# agent-hotline

Open-source, model-agnostic Claude Code skill for turning two terminals into a **Builder + Architect** team.

Always-on collaboration between local and cloud agents.

Built for people who want a lightweight way to run a local model in one terminal and a cloud model in another, with a shared protocol instead of copy/paste.

## Install

1. Add this repo to your Claude Code plugin marketplace:
   ```
   /plugin marketplace add multica-ai/agent-hotline
   ```
2. Install the skill in **both** terminals:
   ```
   /plugin install agent-hotline-skills@agent-hotline
   ```

## What it gives you
- A local **Builder** that can run on your GPU, CPU, or any self-hosted setup.
- A cloud **Architect** that plans, reviews, and clears blockers.
- A tiny shared protocol through `agent-hotline read`, `write`, and `clear`.

## Quick start
1. **Architect** writes a scoped plan and hands off:
   ```
   agent-hotline write "Plan ready. Implement feature X in src/... then run tests."
   ```
2. **Builder** reads, implements, and updates status:
   ```
   agent-hotline read
   agent-hotline write "Implemented X. Tests: <command>. Needs review."
   ```
3. **Architect** reviews and clears:
   ```
   agent-hotline clear
   ```

## How it works
`agent-hotline` stores handoff state in a local `.hotline.json` file so both terminals stay synchronized without copy/paste.

## Learn more
- **[FRAMEWORK.md](./docs/FRAMEWORK.md)** — architecture, setup examples, workflow philosophy
- **[Copilot instructions](./.github/copilot-instructions.md)** — for contributors

## Community
- Open an issue for ideas, bug reports, and workflow requests.
- Send a PR with improvements to the skill, plugin metadata, or documentation.
- Fork it and adapt the Builder/Architect split to your own local model setup.
