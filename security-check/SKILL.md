---
name: security-check
description: "Scans projects for secrets, vulnerabilities, and unsafe patterns"
user_invocable: true
version: "1.0.0"
source: custom
---

# Security Check

Run a security audit on one or more projects. Catch secrets, vulnerable
dependencies, and unsafe patterns before they reach production.

## Scope

- **Single project**: Run against the current working directory
- **All projects**: When user says "all projects" or runs from `~/Desktop/code/`
- **Pre-deploy gate**: Auto-run before `/deploy` if no recent check exists

## Checks

Run these in order. Stop and report immediately if a critical issue is found.

### 1. Secret Scan (CRITICAL)

Search source files (not node_modules/target/) for:
- API keys matching known provider prefixes (sk-, AKIA, ghp_, xox-, AIza)
- Generic credential assignments (password/secret/token = "literal")
- PEM-encoded private keys
- Connection strings with embedded credentials

Use grep with --exclude-dir for node_modules, target, and .git.
Only scan source extensions: ts, tsx, js, jsx, html, rs, toml, json.
Exclude lock files.

If any match is in actual source (not a library type def), flag CRITICAL.

### 2. Environment File Check

- Verify .env* files (excluding .env.example) are listed in .gitignore
- Check if any .env files are tracked: `git ls-files '*.env*'`
- Check git history for past commits of env files
- If found in history, recommend BFG Repo-Cleaner

### 3. Dependency Audit

- **Node projects**: `npm audit --omit=dev`
- **Rust projects**: `cargo audit` (install if missing)
- Severity: critical/high = blocker, moderate/low = warning

### 4. Gitignore Check

Verify .gitignore exists and covers:
- node_modules/, target/, .env*, *.pem, *.key
- IDE folders: .idea/, .vscode/ (unless intentional)
- OS files: .DS_Store, Thumbs.db

If .gitignore is missing entirely, flag and offer to create one.

### 5. Unsafe Code Patterns

Scan for OWASP Top 10 patterns:
- XSS vectors: innerHTML assignment, unsanitized HTML rendering
- Code injection: dynamic code execution, template injection
- SQL injection: string concatenation in queries
- Overly permissive CORS in production config

Flag as warnings, not blockers. Context matters for these.

## Output Format

```
## Security Report: [project-name]
Date: [date]

### Critical (must fix before deploy)
- [issue]

### Warnings (review recommended)
- [issue]

### Passed
- [check]

### Recommendations
- [actionable next step]
```

## Logging

After each run, append to `~/.claude/security-log.jsonl`:

```json
{"project": "name", "date": "2026-03-14", "critical": 0, "warnings": 2, "passed": 3}
```

This enables:
- Tracking which projects were audited recently
- Pre-deploy gating (skip if audited in last 7 days with no changes)
- Trend tracking

## Chain Hints

- If running before /deploy, block on any critical issue
- After fixes, re-run the relevant check to confirm
- Consider /self-eval after applying security fixes

## Rules

1. **Never display secret values.** Show file:line but mask the value.
2. **Don't auto-fix dependency vulns with --force.** Let the user decide.
3. **False positives are OK.** Flag and note when likely false positive.
4. **Check actual .gitignore.** Some projects intentionally track files.
5. **Source files only.** Never scan node_modules/, target/, or .git/.
