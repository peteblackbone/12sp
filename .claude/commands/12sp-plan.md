---
description: 12sp planning — requirements review, UX (if needed), and technical design, saved to .12sp/plan.md for approval. Changes no project code. Hands off to /12sp.
argument-hint: <task description>
---

Read and follow `.claude/12sp/rules.md`.

Task: $ARGUMENTS

## Stages
1. **Preflight** — read `CLAUDE.md`. No baseline needed (no code changes).
2. **requirements-reviewer** — if not ready for design, stop and ask the user the blocking questions; continue after they answer.
3. **ui-designer (ux mode)** — only if requirements-reviewer says the change touches user-facing UI **and** there's no approved `.12sp/ui.md` for this task. If an approved `ui.md` exists, use it instead and skip this stage.
4. **solution-architect** — pass everything above. If it proposes a new dependency, an upgrade, or anything marked "requires approval", list it under "Pending approvals".
5. **Write `.12sp/plan.md`**, first line `Status: draft`, with:
   - Task and requirements summary, acceptance criteria, out of scope, open questions
   - Project constraints
   - UI design: reference to approved `ui.md`, or the ux-mode output, or "not applicable"
   - Technical design (solution-architect output)
   - Recommended stages for the build run (from requirements-reviewer's "Relevant downstream stages"), including whether characterization tests are needed
   - Pending approvals
6. **Approve** — ask the user to review `.12sp/plan.md` (they can edit it directly). Answer questions and revise if asked. When they explicitly approve, including any pending approvals, set the first line to `Status: approved`.

Wrap up per the rules. Suggested next step: `/12sp <task>`.
