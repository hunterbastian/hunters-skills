---
name: deploy
description: "Builds, commits, and pushes project to GitHub"
user_invocable: true
version: "1.0.0"
source: custom
---

# Deploy to GitHub

Build-check, commit, and push the current project to GitHub.

## Steps

1. **Security check.** Run `/security-check` first. If any critical issues are found, block the deploy and report them. If the project was audited within the last 7 days (check `~/.claude/security-log.jsonl`), skip this step.
2. **Identify the project.** Use the current working directory. Confirm the project name with the user if ambiguous.
3. **Build check.** Auto-detect and run the right command:
   - Rust: `cargo check`
   - TypeScript/Next.js: `npx tsc --noEmit` or `npm run build`
   - Vite: `npm run build`
   - HTML-only: skip build check
   - If a project-level CLAUDE.md exists, check it for build instructions.
4. **If build fails**, report the errors and stop. Do not push broken code.
5. **Stage changes.** Run `git status` and `git diff` to show what will be committed. Stage relevant files (avoid secrets, .env, node_modules, target/).
6. **Commit.** Write a concise commit message summarizing the changes. Use the repo's existing commit style if visible in `git log`.
7. **Push.** Push to the current branch on origin. If no upstream is set, push with `-u`.
8. **Report.** Show the commit hash and confirm the push succeeded.

## Rules

- Never force push.
- Never push to main/master without confirming with the user.
- Never commit .env, credentials, or secrets.
- If there are no changes to commit, say so and stop.
- Always show `git status` before committing so the user can review.
