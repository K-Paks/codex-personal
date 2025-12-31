---
plan_id: RESEARCH_PLAN_N
created_at: [ISO timestamp with timezone]
topic: "[Short name of the research topic]"
status: planned
results_dir: mds/<keyword>/research/completed/
results_filename_hint: "RESEARCH_PLAN_N__kebab-topic.md"
constraints:
  - no_subagents
  - document_only_no_recommendations
---

## Goal
[What the executing agent should be able to answer / produce]

## Scope
- **In scope**: [files/dirs/systems]
- **Out of scope**: [explicit exclusions]

## Key Threads (optional)
- [Thread A]
- [Thread B]

## Investigation Steps (checklist)
- [ ] Step 1 (include exact file paths and/or search terms)
- [ ] Step 2
- [ ] Step 3

## Evidence to Capture (required)
- File paths + line ranges for key code
- Request/response shapes for relevant API routes (if applicable)
- Data model entities + relations (if applicable)
- Any config/env assumptions discovered (if applicable)
- Example data (if applicable and reachable using MCP/CLI tools)

## Output Format
- A concise write-up that answers the Research Question
- A “Code References” section listing `path:line-range` pointers
- A short “Open Questions” section if anything can’t be verified

## Done Criteria
- The Research Question is answered with concrete references to code locations.
- All claims are tied to specific files/lines or observed runtime behavior (if executed).
