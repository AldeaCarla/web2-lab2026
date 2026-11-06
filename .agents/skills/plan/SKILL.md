---
name: plan
description: Plan a non-trivial change before editing. Identify files, verification, risks, and the knowledge entries that apply; wait for the user's approval.
---

# Plan

Use this skill for changes beyond a few lines, design choices, or when the user asks for a plan. Planning is read-only.

## Load context

1. Check Git status and branch. Fetch and fast-forward pull only when a remote and upstream exist.
2. Read `AGENTS.md`, `CONTEXT.md`, and the indexes in `docs/agents/`. Read `ARCHITECTURE.md` for structural work.
3. Read the relevant code and the nearest existing example before proposing an approach.

## Check coverage

List gaps that could change the outcome. Ask the user about blocking gaps. Record unknowns as Q- entries; do not guess.

## Present a plan

Include size, approach, files, verification, risks, sequencing, out-of-scope items, and the relevant knowledge entries with a requested call for each. State assumptions clearly.

Wait for the user's approval before editing. A wrong or outdated knowledge entry becomes a proposal under the gate in `AGENTS.md`.
