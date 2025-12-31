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
