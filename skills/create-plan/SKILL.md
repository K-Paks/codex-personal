---
name: create-plan
description: Create detailed implementation plans through an interactive, iterative process based on completed research. Use when the user requests /create-plan or asks to draft an implementation plan from a ticket and research artifacts.
---

# Implementation Plan

Create detailed implementation plans through an interactive, iterative process. Be skeptical, thorough, and work collaboratively with the user to produce high-quality technical specifications. **You MUST ask clarifying questions in a loop until you are 100% sure about all aspects of the plan—do not proceed with assumptions.** 

**Assume research is already complete.**
- Do not spawn subagents.
- Do not run a broad “research phase” as part of planning.
- Base the plan on the provided ticket/context and existing research artifacts.

## Initial Response

When this command is invoked:

1. **Check if parameters were provided**:
   - If a file path or ticket reference was provided as a parameter, skip the default message
   - Immediately read any provided files fully
   - Begin the planning process (assume research has already been completed)

2. **If no parameters provided**, respond with:
```
I'll help you create a detailed implementation plan. Let me start by understanding what we're building.

Please provide:
1. The task/ticket description (or reference to a ticket file)
2. Any relevant context, constraints, or specific requirements
3. Links to research results / discoveries already completed (or paste the key findings)

I'll analyze this information and work with you to create a comprehensive plan.
```

Then wait for the user's input.

## Process Steps

### Step 1: Inputs Review & Initial Analysis (research assumed complete)

1. **Read all mentioned files immediately and fully**:
   - Ticket files (e.g., `mds/<keyword>/plan/request/eng_1234.md`)
   - Research documents (e.g., `mds/<keyword>/research/completed/`)
   - Related implementation plans
   - Any JSON/data files mentioned
   - **NEVER** read files partially - if a file is mentioned, read it completely

2. **Confirm research inputs are sufficient**:
   - If research results are referenced (e.g., findings docs, notes, prior plan), ensure they include:
     - Current-state behavior (what exists today)
     - Key constraints and conventions to follow
     - Relevant file paths / endpoints / models (with references)
   - If anything essential is missing, ask the user to provide the missing research artifact(s) or pointers.

3. **Analyze and verify understanding**:
   - Cross-reference the requirements with the provided research findings
   - Identify any discrepancies or misunderstandings
   - Note assumptions that need verification
   - Determine true scope based on the documented current state

4. **Present informed understanding and focused questions**:
   ```
   Based on the ticket and the research findings provided, I understand we need to [accurate summary].

   I've found that:
   - [Current implementation detail with file:line reference from research]
   - [Relevant pattern or constraint discovered]
   - [Potential complexity or edge case identified]

   Questions the available research doesn't answer:
   - [Specific technical question that requires human judgment]
   - [Business logic clarification]
   - [Design preference that affects implementation]
   ```

   Only ask questions that you genuinely cannot answer from the provided materials. **Keep asking questions in a loop until you are 100% sure about all aspects—do not proceed with assumptions.**

### Step 2: Clarifications & Decisions (no new research phase)

After getting initial clarifications:

1. **If the user corrects any misunderstanding**:
   - Do not just accept the correction
   - Ask for (or locate) the specific evidence that supports the correction (file path + line range, or a research doc excerpt)
   - Only proceed once the correction is grounded in concrete references

2. **Create a planning todo list** to track planning tasks

3. **Present findings and design options**:
   ```
   Based on the existing research, here's what we know:

   **Current State:**
   - [Key discovery about existing code]
   - [Pattern or convention to follow]

   **Design Options:**
   1. [Option A] - [pros/cons]
   2. [Option B] - [pros/cons]

   **Open Questions:**
   - [Technical uncertainty]
   - [Design decision needed]

   Which approach aligns best with your vision?
   ```

### Step 3: Plan Structure Development

Once aligned on approach:

1. **Create initial plan outline**:
   ```
   Here's my proposed plan structure:

   ## Overview
   [1-2 sentence summary]

   ## Implementation Phases:
   1. [Phase name] - [what it accomplishes]
   2. [Phase name] - [what it accomplishes]
   3. [Phase name] - [what it accomplishes]

   Does this phasing make sense? Should I adjust the order or granularity?
   ```

2. **Get feedback on structure** before writing details

### Step 4: Detailed Plan Writing

After structure approval:

1. **Write the plan** to `mds/<keyword>/plan/completed/plan.md`
   - Directory: `mds/<keyword>/plan/completed/` (create if missing)
2. **Use this template structure**:

````markdown
# [Feature/Task Name] Implementation Plan

## Overview

[Brief description of what we're implementing and why]

## Current State Analysis

[What exists now, what's missing, key constraints discovered]

## Desired End State

[A specification of the desired end state after this plan is complete, and how to verify it]

### Key Discoveries:
- [Important finding with file:line reference]
- [Pattern to follow]
- [Constraint to work within]

## What We're NOT Doing

[Explicitly list out-of-scope items to prevent scope creep]

## Implementation Approach

[High-level strategy and reasoning]

## Phase 1: [Descriptive Name]

### Overview
[What this phase accomplishes]

### Changes Required:

#### 1. [Component/File Group]
**File**: `path/to/file.ext`
**Changes**: [Summary of changes]

```[language]
// Specific code to add/modify
```

### Success Criteria:

#### Automated Verification:
- [ ] Build passes: `pnpm build`
- [ ] Linting passes: `pnpm lint`
- [ ] <optional> PlayWright automated tests verified created features (only if the changes are UI-based)

#### Manual Verification:
- [ ] Feature works as expected when tested via UI
- [ ] Edge case handling verified manually
- [ ] No regressions in related features

**Implementation Note**: After completing this phase and all automated verification passes, pause here for manual confirmation from the human that the manual testing was successful before proceeding to the next phase.

---

## Phase 2: [Descriptive Name]

[Similar structure with both automated and manual success criteria...]

---

## Testing Strategy

### Integration Tests:
- [End-to-end scenarios]

### Manual Testing Steps:
1. [Specific step to verify feature]
2. [Another verification step]
3. [Edge case to test manually]

## Performance Considerations

[Any performance implications or optimizations needed]

## Migration Notes

[If applicable, how to handle existing data/systems]

## References

- Related research: `mds/<keyword>/research/completed/[relevant].md`
- Similar implementation: `[file:line]`
````

### Step 5: Sync and Review

1. **Present the draft plan location**:
   ```
   I've created the initial implementation plan at:
   `mds/<keyword>/plan/completed/plan.md`

   Please review it and let me know:
   - Are the phases properly scoped?
   - Are the success criteria specific enough?
   - Any technical details that need adjustment?
   - Missing edge cases or considerations?
   ```

2. **Iterate based on feedback** - be ready to:
   - Add missing phases
   - Adjust technical approach
   - Clarify success criteria (both automated and manual)
   - Add/remove scope items

3. **Continue refining** until the user is satisfied

## Important Guidelines

1. **Be Skeptical**:
   - Question vague requirements
   - Identify potential issues early
   - Ask "why" and "what about"
   - Don't assume - verify with code

2. **Be Interactive**:
   - Don't write the full plan in one shot
   - Get buy-in at each major step
   - Allow course corrections
   - Work collaboratively

3. **Be Thorough**:
   - Read all context files completely before planning
   - Use existing research artifacts and explicit file:line references instead of running a new research phase
   - Include specific file paths and line numbers
   - Write measurable success criteria with clear automated vs manual distinction

4. **Be Practical**:
   - Focus on incremental, testable changes
   - Consider migration and rollback
   - Think about edge cases
   - Include "what we're NOT doing"

5. **Track Progress**:
   - Track planning tasks while drafting
   - Update tasks as you complete planning
   - Mark planning tasks complete when done

6. **No Open Questions in Final Plan**:
   - If you encounter open questions during planning, stop
   - Ask for clarification immediately (or request the missing research artifact/pointer)
   - Do not write the plan with unresolved questions
   - The implementation plan must be complete and actionable
   - Every decision must be made before finalizing the plan

## Success Criteria Guidelines

**Always separate success criteria into two categories:**

1. **Automated Verification** (can be run by execution agents):
   - Specific files that should exist
   - Code compilation/type checking
   - Automated test suites

2. **Manual Verification** (requires human testing):
   - UI/UX functionality
   - Performance under real conditions
   - Edge cases that are hard to automate
   - User acceptance criteria

**Format example:**
```markdown
### Success Criteria:

#### Automated Verification:
- [ ] No linting errors: `pnpm lint`
- [ ] Project builds: `pnpm build`
- [ ] API endpoint returns 200: `curl localhost:8080/api/new-endpoint`
- [ ] UI flow verified with PlayWright MCP

#### Manual Verification:
- [ ] New feature appears correctly in the UI
- [ ] Performance is acceptable with 1000+ items
- [ ] Error messages are user-friendly
- [ ] Feature works correctly on mobile devices
```

## Common Patterns

### For New Features:
- Confirm existing patterns from the provided research
- Start with data model
- Build backend logic
- Add API endpoints
- Implement UI last

### For Refactoring:
- Document current behavior
- Plan incremental changes
- Maintain backwards compatibility
- Include migration strategy
