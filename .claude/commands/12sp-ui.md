---
description: 12sp UI design — requirements check, then UX + visual direction + a clickable HTML mockup in .12sp/mockups/. Changes no project code. Hands off to /12sp-plan and /12sp.
argument-hint: <feature or screens to design>
---

Read and follow `.claude/12sp/rules.md`.

Task: $ARGUMENTS

## Stages
1. **Preflight** — read `CLAUDE.md`. No git check or baseline needed (only `.12sp/` is written).
2. **requirements-reviewer (light)** — ask it to focus on what the screens must do: summary, acceptance criteria, user-facing edge cases, and open questions. If it's not ready for design, stop and ask the user the blocking questions.
3. **ui-designer in visual mode** — pass the task, constraints, and requirements output. It writes `.12sp/ui.md` (`Status: draft`) and the mockup in `.12sp/mockups/`.
4. **Review loop** — tell the user to open `.12sp/mockups/index.html` in a browser and read `.12sp/ui.md`. Collect their feedback and re-run ui-designer (visual mode, with the feedback and the current files) until they're happy.
5. **Approve** — when the user explicitly approves, set the first line of `ui.md` to `Status: approved`.

Wrap up per the rules. Suggested next step: `/12sp-plan <task>` (technical design), or `/12sp <task>` to plan and build in one go.
