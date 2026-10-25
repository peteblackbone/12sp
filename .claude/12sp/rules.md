# 12sp shared rules

Every /12sp command reads and follows this file. Agents live in `.claude/agents/` and are run via the Agent tool.

## Hard rules — always
1. **Never push, merge, or deploy anything, or trigger a CI/CD pipeline that would.** Every stage produces a diff, plan, design, or document for the human to review and act on.
2. **Never add, upgrade, or remove dependencies, or change the language/runtime version or project-wide compiler settings**, unless solution-architect proposed it and the user explicitly approved it.
3. **Only new problems block.** A new build/lint/test failure compared to the baseline, a "Fix required before merge? yes" finding about *this change*, or acceptance-reviewer's "reject" / "accept with concerns" blocks progression. Pre-existing failures and legacy debt never block — collect them for the wrap-up.
4. **When something blocks, stop**, mark it blocked in the status file, and surface it to the user. Don't decide on the user's behalf that it's minor.
5. **Design-only commands (/12sp-ui, /12sp-plan, /12sp-review, /12sp-post) never modify project files** — they write only inside `.12sp/`.

## Preflight
Each command says which of these it needs:
- **Read `CLAUDE.md`** in the project root. If missing, tell the user (suggest creating it from `CLAUDE.template.md`) and continue with constraints inferred from project files, marked as inferred.
- **Check git:** run `git status`. If there are uncommitted changes, warn the user and ask whether to continue (recommend committing/stashing and working on a separate branch).
- **Baseline:** run the build and test commands (from `CLAUDE.md`, or the obvious ones for the stack) *before changing anything*, and record what builds, what passes, and what already fails. If nothing can be built or tested, record that.

## Passing context between stages
Pass to every stage: the original task, project constraints (from `CLAUDE.md` / requirements-reviewer), the baseline (if taken), any approved artifacts, and the outputs of prior stages in this run. Tell ui-designer and test-writer which mode they're in.

## Artifacts and handoff (`.12sp/` folder in the project root)
| File | Written by | Used by |
|---|---|---|
| `.12sp/ui.md` + `.12sp/mockups/` | /12sp-ui | /12sp-plan, /12sp, developer, acceptance-reviewer |
| `.12sp/plan.md` | /12sp-plan | /12sp, /12sp-review |
| `.12sp/status.md` | every command | the user |
| `.12sp/review.md` | /12sp-review | the user |

- The first line of `ui.md` and `plan.md` is `Status: draft` or `Status: approved`. Change it to `approved` **only** when the user explicitly approves in chat.
- A command uses an artifact only if it's **approved and about the same task**. If an approved artifact exists for a *different* task, ask the user whether to archive it before continuing.
- **Archive** = move the current `ui.md`, `plan.md`, `review.md`, `status.md`, and `mockups/` into `.12sp/archive/<YYYY-MM-DD>-<short-task-slug>/`. Do this when the user starts a new, unrelated task, or after a full run completes and the user confirms.

## Status file (`.12sp/status.md`)
Overwrite at the start of each command run (archive first if it belongs to a different task). Plain Markdown:
- Header: command, task, date/time, overall state (running / waiting for user / blocked / complete)
- Table of the stages this command runs, each with status (pending / running / done / skipped / blocked) and a one-line verdict
- "Baseline" section (if taken)
- "Pending approvals" section
- One section per completed stage with its full output, newest last

Update it after every stage, and give a one-sentence chat update each time.

## Wrap-up (every command)
End with a short chat summary: what was produced or changed, how it was verified, what's still open (approvals, blocking items, pre-existing failures, legacy debt), and the suggested next command. For commands that change code, remind the user to review the diff and commit/push/deploy themselves.
