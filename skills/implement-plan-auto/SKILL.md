---
name: implement-plan-auto
description: Implement approved technical plans from thoughts/shared/plans without verification. Use when the user provides a plan path or asks to execute a plan and track phase progress and checks.
---

# Implement Plan

Implement an approved technical plan. The plan contains phases with specific changes and success criteria.

## Getting Started

When given a plan path:
- Read the plan completely and check for any existing checkmarks (`- [x]`)
- Read the original ticket and all files mentioned in the plan
- Read files fully - never use limit/offset parameters, you need complete context
- Think deeply about how the pieces fit together
- Create a todo list to track your progress
- Start implementing if you understand what needs to be done

If no plan path provided, ask for one.

## Implementation Philosophy

Plans are carefully designed, but reality can be messy. Your job is to:
- Follow the plan's intent while adapting to what you find
- Implement each phase fully before moving to the next
- Verify your work makes sense in the broader codebase context
- Update checkboxes in the plan as you complete sections

**FORBIDDEN**: You must NEVER run database migrations or make direct database changes. When a migration is needed:
1. Create the migration file if required
2. Stop and prompt the user to run the migration manually
3. Wait for confirmation before continuing
4. Document the migration in the PR description's "Before Merging" section

When things don't match the plan exactly, think about why and communicate clearly. The plan is your guide, but your judgment matters too.

If you encounter a mismatch:
- Stop and think deeply about why the plan can't be followed
- Present the issue clearly:
  ```
  Issue in Phase [N]:
  Expected: [what the plan says]
  Found: [actual situation]
  Why this matters: [explanation]

  How should I proceed?
  ```

## Verification Approach

After implementing a phase:
- Run the success criteria checks (if specified in the task)
- Fix any issues before proceeding
- Update your progress in both the plan and your todos
- Check off completed items in the plan file itself using Edit
- Commit the phase: After completing all verification for a phase, commit the code (excluding lock files):
  ```
  git add -A && git reset HEAD pnpm-lock.yaml package.json 2>/dev/null; git commit -m "phase N - <relevant message describing what was implemented>"
  ```
  Example: `git commit -m "phase 1 - add user authentication endpoints"`

Continue automatically to the next phase after committing. Do not wait for human input between phases.

## Completing the Plan

After all phases are complete:

1. **Track what you did**: Keep a running log of:
   - All changes made (files created, modified, deleted)
   - All tests performed and their results
   - Any migrations or setup steps required

2. **Create a Pull Request** using the `gh` CLI:
   ```
   gh pr create --title "<plan name or feature summary>" --body "<PR description>"
   ```

3. **PR Description Format**: The body must include:
   ```markdown
   ## Changes Made
   - [List all changes by phase]
   - Phase 1: <what was done>
   - Phase 2: <what was done>
   - ...

   ## Tests Performed
   - [List all automated tests run and results]
   - [List any manual verification done]

   ## Before Merging
   ### Commands to Run
   ```bash
   # Any setup or build commands needed
   ```

   ### Migrations
   - [List any database migrations or data changes required]

   ### Manual Tests
   - [Step-by-step manual testing instructions]
   - [Expected outcomes for each test]
   ```

Do not wait for human input - create the PR automatically when all phases are complete.

## If You Get Stuck

When something isn't working as expected:
- First, make sure you've read and understood all the relevant code
- Consider if the codebase has evolved since the plan was written
- Present the mismatch clearly and ask for guidance

Use sub-tasks sparingly - mainly for targeted debugging or exploring unfamiliar territory.

## Resuming Work

If the plan has existing checkmarks:
- Trust that completed work is done
- Pick up from the first unchecked item
- Verify previous work only if something seems off

Remember: You're implementing a solution, not just checking boxes. Keep the end goal in mind and maintain forward momentum.
