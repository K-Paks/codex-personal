---
name: run-research
description: Execute a RESEARCH_PLAN_N.md to document the existing codebase (no recommendations). Use when given a research plan file path to run its investigation steps, read files/search code, and write the results file specified by the plan.
---

# Run Research

Execute a research plan end-to-end and write the results file the plan specifies.

## Inputs
- A path to a `RESEARCH_PLAN_N.md` file. The plan path is provided at invocation time.

## Initial response (if no plan path provided)
When invoked without a plan path, you MUST respond with:
```
I'm ready to execute a research plan. Please provide the path to a RESEARCH_PLAN_N.md file (e.g., mds/<keyword>/research/request/RESEARCH_PLAN_1.md).
```
Then wait for the user to provide the plan path.

## Constraints
- Document what exists; do not recommend changes or critique.
- Do not perform root cause analysis unless explicitly requested in the plan.
- Do not use subagents.
- Follow any extra constraints listed in the plan frontmatter.

## Workflow
1) Read the plan file fully.
2) Extract key fields from frontmatter: `plan_id`, `topic`, `results_dir`, `results_filename_hint`, and `constraints`.
3) Execute the plan’s Investigation Steps using targeted file reads and searches.
   - Prefer precise file reads for mentioned paths.
   - Use `rg` for fast codebase search.
4) Gather evidence required by the plan (file paths + line ranges, API shapes, data models, config assumptions, example data if applicable).
5) Write the results file to `mds/<keyword>/research/completed/` using the `results_filename_hint` from the plan.
   - Create the `completed/` directory if missing.
   - If a file with the same name exists, append a suffix `__2`, `__3`, etc.
6) Summarize what you wrote and point to the results file path.

## Results format
Use this template and keep it consistent with the plan’s constraints:

```markdown
---
plan_id: RESEARCH_PLAN_N
plan_file: mds/<keyword>/research/request/RESEARCH_PLAN_N.md
executed_at: [ISO timestamp with timezone]
topic: "[Same topic as plan (or refined)]"
status: complete
constraints:
  - document_only_no_recommendations
---

## Research Goal
[Paste the original research goal verbatim]

## Summary
[High-level description of what exists, answering the question]

## Detailed Findings
### [Area 1]
- What exists, where it exists, how it works.
- Include code references like `path/to/file.ts:10-42`.

### [Area 2]
...

## Code References
- `path/to/file.ts:10-42` - What this shows
- `path/to/other.ts:5-18` - What this shows

## Data / API Shapes (if applicable)
- [Request/response examples, DB entities/relations]

## Historical Context (if applicable)
- `mds/...` or `thoughts/...` (if present in repo)

## Example data
- row from database (reached with, for example, Supabase MCP)
- PR from Github (reached with gh CLI)

## Open Questions
- [Anything that could not be confirmed from the codebase]
```

## Reminder
Be a documentarian: describe what is, where it is, and how it interacts. No recommendations.
