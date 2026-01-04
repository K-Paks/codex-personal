---
name: oneshot-research
description: Create a single research request document for another agent to execute. Use when you need to spec out one focused research topic without creating multiple plans.
---

# Oneshot Research

Create a single, well-specified research document that another agent can pick up and execute to document the existing codebase.

## When to Use

Use this skill when:
- You have a single, focused research question
- You want to hand off research to another agent
- You don't need the overhead of multiple research plans

For complex research with multiple angles, use `plan-research` instead.

## Constraints
- Document what exists; do not recommend changes or critique.
- Do not perform root cause analysis unless explicitly requested.
- Do not use subagents.
- Create only ONE research document per invocation.

## Initial Response

When invoked, respond with:
```
I'll create a research request document for another agent to execute. What topic or question do you want researched?
```
Then wait for the user's research query.

## Workflow

1. **Understand the question**: Read any mentioned files to ground understanding.
2. **Scope it tightly**: Define clear boundaries - what's in scope and out of scope.
3. **Create the research request**: Write a single document to `<repo-root>/mds/<keyword>/research/request/RESEARCH_REQUEST.md`
   - `<keyword>` should be a short, identifiable name for the research area
4. **Present the document**: Show what you created and confirm it captures the intent.

## Research Request Template

```markdown
---
request_id: RESEARCH_REQUEST
created_at: [ISO timestamp with timezone]
topic: "[Short name of the research topic]"
status: pending
results_dir: mds/<keyword>/research/completed/
results_filename: "RESEARCH_RESULTS.md"
constraints:
  - document_only_no_recommendations
---

## Research Goal
[Clear statement of what the executing agent should answer or produce]

## Context
[Background information to help the executing agent understand why this research matters]

## Scope
- **In scope**: [files/dirs/systems to investigate]
- **Out of scope**: [explicit exclusions]

## Investigation Steps
- [ ] Step 1 (include exact file paths and/or search terms where known)
- [ ] Step 2
- [ ] Step 3

## Evidence to Capture
- File paths + line ranges for key code
- Request/response shapes for relevant API routes (if applicable)
- Data model entities + relations (if applicable)
- Config/env assumptions (if applicable)
- Example data from DB or APIs (if reachable)

## Expected Output
- A concise write-up answering the research goal
- A "Code References" section with `path:line-range` pointers
- An "Open Questions" section for anything unverifiable

## Done Criteria
- The research goal is answered with concrete code references
- All claims are tied to specific files/lines
```

## Results Template (for executing agent)

The agent executing this research should write results to `mds/<keyword>/research/completed/RESEARCH_RESULTS.md`:

```markdown
---
request_id: RESEARCH_REQUEST
request_file: mds/<keyword>/research/request/RESEARCH_REQUEST.md
executed_at: [ISO timestamp with timezone]
topic: "[Topic from request]"
status: complete
constraints:
  - document_only_no_recommendations
---

## Research Goal
[Paste original goal]

## Summary
[High-level answer to the research question]

## Detailed Findings
### [Area 1]
- What exists, where, how it works
- Code references: `path/to/file.ts:10-42`

### [Area 2]
...

## Code References
- `path/to/file.ts:10-42` - What this shows
- `path/to/other.ts:5-18` - What this shows

## Data / API Shapes (if applicable)
- Request/response examples, DB entities

## Example Data (if applicable)
- DB rows, API responses, etc.

## Open Questions
- [Anything that couldn't be confirmed]
```

## Reminder
Be a documentarian: describe what is, where it is, and how it interacts. No recommendations.

