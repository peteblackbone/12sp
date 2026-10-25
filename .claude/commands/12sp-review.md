---
description: 12sp review — runs the review stages (security, compliance if needed, code quality, acceptance) on existing changes, without editing code. Default target is uncommitted changes plus the current branch vs main. Results go to .12sp/review.md.
argument-hint: [branch | commit range] (optional)
---

Read and follow `.claude/12sp/rules.md`. This command never edits project files.

Target: $ARGUMENTS

## Stages
1. **Preflight** — read `CLAUDE.md` and run `git status`.
2. **Resolve the target:**
   - No argument → uncommitted changes plus the current branch compared to its base (`main`, or `master`/`develop` if that's what the repo uses)
   - A branch name → that branch vs the base branch
   - A commit range (e.g. `HEAD~3..HEAD`) → that range
   Show the user the files and commit count in scope. If the diff is empty, say so and stop.
3. **Find the requirement** — use an approved `.12sp/plan.md` for this change if one exists; otherwise ask the user in one or two sentences what the change was supposed to do (acceptance-reviewer needs it). If they'd rather skip it, run without acceptance-reviewer.
4. **Run the existing tests** once, report results, and note which failures relate to changed files. Don't write tests.
5. **Reviewers**, scoped to the target diff:
   - `security-reviewer`
   - `compliance-reviewer` — only if the diff collects, stores, or transmits personal data in a new way
   - `code-quality-reviewer`
   - `acceptance-reviewer` — if a requirement is available
6. **Write `.12sp/review.md`** — overall verdict, blocking findings (fix before merge), non-blocking findings, pre-existing issues/legacy debt, and test results.

Wrap up per the rules. Suggested next step for blocking findings: `/12sp-lite <fix>`.
