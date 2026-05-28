# Copilot instructions

## Project overview
- This repository documents the `agent-hotline` workflow: an open-source, model-agnostic Builder + Architect setup for Claude Code terminals.
- Two terminals work on the same workspace: a local Builder on any self-hosted model/runtime and a cloud Architect for planning, review, and unblocker work.

## High-level architecture
- Coordination is done through the `agent-hotline` CLI, which writes/reads shared state in `.hotline.json` using `write`, `read`, and `clear` commands.
- The intended workflow is: Architect plans → `agent-hotline write` handoff → Builder implements → Architect reviews.
- The repo also ships as a Claude Code skill/plugin so the same handoff protocol can be installed in both terminals.

## Key conventions
- Use `agent-hotline write/read` for cross-agent handoffs instead of ad hoc notes.
- Keep the Builder/Architect role split intact when proposing workflow changes or edits to the documentation.
- Prefer model-agnostic wording unless a file is explicitly documenting an example setup.
