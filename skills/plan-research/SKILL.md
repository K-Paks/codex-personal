---
name: plan-research
description: Create research plan files for other agents to document the existing codebase (no recommendations or changes). Use when asked to draft RESEARCH_PLAN_N.md files from a research question or angle, including scoping, investigation steps, and output requirements.
---

# Plan Research

Create research plan files that other agents can execute later to document what exists in the codebase.

## Constraints
- Document what exists today; do not recommend changes or critique.
- Do not perform root cause analysis unless explicitly requested.
- Do not use subagents.

## Initial response
When invoked, respond with:
```
I'm ready to research the codebase. Please provide your research question or area of interest, and I'll analyze it thoroughly by exploring relevant components and connections.
```
Then wait for the user's research query.

## Workflow after receiving the research query
1) Read any directly mentioned files first (full file reads).
2) Decompose the question into composable research areas.
3) Create one or more `RESEARCH_PLAN_N.md` files in `<repo-root>/mds/<keyword>/research/request/`, where <keyword> is an identifiable, shortened problem we're solving
4) Do minimal validation with targeted searches or minimal file reads to ground the plan.
5) Present a plan inventory summary and ask for prioritization or splitting/merging.

## Plan files
- Naming: `RESEARCH_PLAN_1.md`, `RESEARCH_PLAN_2.md`, ... (do not rename existing files).
- Prefer 2–5 focused plans if the question naturally splits.
- Use the template in `references/plan-template.md`.

## Results guidance for executing agents
- Results directory: `<repo-root>/mds/<keyword>/research/completed/` (create if missing).
- Filename: `RESEARCH_PLAN_N__kebab-topic.md` (append `__2`, `__3` if needed).
- Use the template in `references/results-template.md`.

## Reminder
Be a documentarian: describe what is, where it is, and how it interacts. No recommendations.
