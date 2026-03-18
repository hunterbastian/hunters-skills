---
name: bug-reproduce-first
description: "Auto-activates when user reports a bug or error"
version: "1.0.0"
source: custom
---

# Bug Reproduce First

When the user reports a bug, do NOT jump straight to fixing it. First
understand and reproduce the problem, then fix it with proof.

## Workflow

1. **Understand the bug.** Read the user's description and identify:
   - What is the expected behavior?
   - What is the actual behavior?
   - What file(s) are likely involved?

2. **Locate the relevant code.** Read the file(s) where the bug lives.
   Understand the current implementation before changing anything.

3. **Reproduce the bug.** Depending on the project:
   - **If the project has a test framework:** Write a failing test that
     captures the exact broken behavior. Run it to confirm it fails.
   - **If it's a browser/UI bug:** Use Playwright to navigate to the
     affected page and verify the issue (take a screenshot, check console
     errors, inspect the DOM snapshot).
   - **If it's a build/runtime error:** Run the build or relevant command
     to see the error output firsthand.

4. **Diagnose the root cause.** Now that you can see the failure, identify
   WHY it's happening. Look for:
   - Off-by-one errors
   - Missing null/undefined checks
   - Wrong variable scope
   - Race conditions
   - Incorrect CSS specificity or ordering
   - State management issues (hooks order, stale closures)

5. **Fix the bug.** Make the minimum change needed to fix the issue.
   Do not refactor surrounding code. Do not add unrelated improvements.

6. **Verify the fix.**
   - If you wrote a test: run it again and confirm it passes.
   - If it's a UI bug: take a new screenshot showing the fix.
   - If it's a build error: run the build again and confirm it succeeds.

7. **Report to the user.** Briefly explain:
   - What caused the bug (1 sentence)
   - What you changed to fix it (1 sentence)
   - How you verified it works (test passed / screenshot / build succeeded)

## Rules

- NEVER skip step 3 (reproduce). If you can't reproduce it, tell the user
  and ask for more information. Do not guess at fixes.
- NEVER make changes beyond the minimum fix. A bug fix is not an invitation
  to refactor, add features, or improve code style.
- If the bug is in a file you haven't read yet, read it FIRST. Do not fix
  code you haven't seen.
- If the fix introduces a new edge case, handle it in the same commit.
  Don't leave known gaps.

## Chain Hints

After fixing a bug, consider:
- **self-eval** — run the self-review checklist on your fix before presenting
- **mobile-check** — if the bug was CSS/layout related, verify at 390×844
- **scope-guard** — make sure the fix didn't include unrequested improvements

## Edge Cases

- If the user says "it's broken" but doesn't specify what: ask them to
  describe the expected vs actual behavior before proceeding.
- If the bug turns out to be a misunderstanding (code is working as
  designed): explain why the current behavior is correct and ask if they
  want to change it.
- If the bug requires a fix across multiple files: fix them all, but
  verify each change independently if possible.
- If you can't write a test (no test framework, UI-only bug): the
  screenshot/console verification in step 3 counts as reproduction.
