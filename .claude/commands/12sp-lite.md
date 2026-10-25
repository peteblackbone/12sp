---
description: 12sp lite — small, low-risk changes (bug fixes, small tweaks): developer, a scoped security check, and tests, with the same safeguards. Escalates to /12sp-plan if the change is bigger. Never pushes, merges, or deploys.
argument-hint: <small task description>
---

Read and follow `.claude/12sp/rules.md`.

Task: $ARGUMENTS

## Stages
1. **Preflight** — read `CLAUDE.md`, check git, and record the baseline.
2. **Scope check** — restate the task and the acceptance criteria you'll check against in two or three sentences. **Escalate** (stop and suggest `/12sp-plan <task>`) if: it needs a new dependency or upgrade; it changes a database schema, stored data, or an external API/contract; it adds or changes user-facing UI beyond a trivial tweak (suggest `/12sp-ui` for that); it handles personal data in a new way; or it's unclear what "done" means. Otherwise state the plan in one line and continue.
3. **developer**
4. **security-reviewer** — unless the change clearly doesn't touch input handling, auth, secrets, or data exposure (say why if skipped).
5. **test-writer** (verification mode) — compare against the baseline.
6. **Wrap up** per the rules — check the result against the criteria from step 2 yourself.
