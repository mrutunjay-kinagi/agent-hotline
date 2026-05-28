---
name: agent-hotline
description: Two-terminal hybrid agent workflow with structured handoffs via the agent-hotline CLI.
---

# Agent Hotline Workflow

Use this skill when operating a **Builder** (local model) and **Architect** (cloud model) in the same workspace.

## Handoff protocol
- **Before starting work**, run `agent-hotline read` to load the latest handoff.
- **When handing off**, run `agent-hotline write "<status>. <what changed>. <next step>."`
- **When a workstream is complete**, run `agent-hotline clear` to reset the state.

## Role expectations
- **Builder:** execute concrete edits, tests, and iterations locally.
- **Architect:** plan, review, and validate high-level structure and risks.
