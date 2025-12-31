---
name: playwright-verify
description: Verify implementation and debug issues using Playwright in this repo (not for writing tests). Use when running Playwright to validate behavior, reproduce issues, or debug failures, including optional creation and cleanup of temporary sample data.
---

# Playwright Verify

Use this skill to run Playwright for verification and debugging only, not for authoring tests.

## Workflow

1) (Optional) Create a helper file describing sample data

Create `mds/PLAYWRIGHT_TEST_DATA.md` only if the user explicitly asks for it. Otherwise skip to step 2.

- Create a temporary helper file named `mds/PLAYWRIGHT_TEST_DATA.md`.
- List the sample entities to be created (clients, exercises, workouts, plans, sessions).
- Include identifying suffixes or IDs so the records can be removed later.
- Keep it small and focused on what the verification actually uses.

Example format:
```
# Playwright Sample Data
- Exercise: "Squat 1714668000-1234"
- Workout: "Strength A 1714668000-1234"
- Plan: "Plan Alpha 1714668000-1234"
- Client: "Alex 1714668000-1234"
```

2) Perform the scheduled Playwright actions
- Run the required Playwright command(s) for the task.
- If a run fails, capture the failure output and debug before retrying.

3) Remove sample data from the database
- Use the helper file to identify records to delete.
- Remove related data in a safe order (sessions/blocks first, then parent entities).
- Confirm removal by rerunning the relevant listing query or UI check.

4) Remove the helper file
- Delete `mds/PLAYWRIGHT_TEST_DATA.md` after cleanup is complete.

## Notes
- The helper file is only required when the user explicitly requests it. Skip steps 1, 3, and 4 otherwise.
- Do not leave temporary data or helper files behind.
