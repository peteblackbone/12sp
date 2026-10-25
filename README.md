# 12sp — 12-step pipeline for Claude Code

12 specialist agents and 6 commands for planning, designing, building, and reviewing changes, tuned for working safely in existing and legacy projects.

## Commands

| Command | What it does | Edits code? |
|---|---|---|
| `/12sp-ui <feature>` | Requirements check → UX + visual direction + clickable HTML mockup | No (writes `.12sp/` only) |
| `/12sp-plan <task>` | Requirements → UX (if needed) → technical design, saved for approval | No |
| `/12sp <task>` | Full pipeline: requirements → design → build → reviews → tests → acceptance → docs → launch plan | Yes |
| `/12sp-lite <task>` | Small fixes: build → security check → tests | Yes |
| `/12sp-review [branch\|range]` | Security, compliance, code-quality, and acceptance review of existing changes | No |
| `/12sp-post <change + data>` | Stage 12: post-launch review once a change has been live | No |

Every command type starts with `/12sp`, so autocomplete shows them together.

### How they connect

```
/12sp-ui  ──►  /12sp-plan  ──►  /12sp  ──►  (you deploy)  ──►  /12sp-post
(screens)      (tech design)    (build)                         (outcome)

/12sp-lite ──escalates──► /12sp-plan        /12sp-review ──fix──► /12sp-lite
```

You can start anywhere. `/12sp-ui` saves `.12sp/ui.md`; `/12sp-plan` saves `.12sp/plan.md`. When you approve them in chat they're marked `Status: approved`, and later commands reuse them and skip those stages. You can also edit them by hand before approving.

## Install (per project)

1. Copy the `.claude` folder into the project root:
   ```
   <project>/.claude/agents/      12 agents
   <project>/.claude/commands/    6 commands
   <project>/.claude/12sp/rules.md   shared rules (all commands read this)
   <project>/.claude/settings.json   optional safety rules
   ```
   If the project already has a `.claude/settings.json`, merge the `deny` list into it instead of overwriting.
2. Copy `CLAUDE.template.md` to the project root as `CLAUDE.md` and fill it in (language/runtime versions, build/test commands, frozen dependencies, quirks, do-not-touch areas). This matters most for legacy projects.
3. Decide whether to commit `.12sp/`. Committing keeps approved plans and designs with the code; otherwise add `.12sp/` to `.gitignore`.
4. Start a new Claude Code session and run `/agents` to confirm the agents are loaded.

If you previously installed the old version globally, remove the old `pipeline.md`, `pipeline-lite.md`, and `3-architect.md` from `C:\Users\<you>\.claude\` so they don't mix with this version.

## Typical uses

```
/12sp-ui Landing page and gallery for an anime-style art site, mobile-first
/12sp-plan Add export-to-CSV on the Orders page (existing WebForms app)
/12sp Add export-to-CSV on the Orders page
/12sp-lite Fix: order total shows 0 when discount is null
/12sp-review feature/csv-export
/12sp-post CSV export. Usage last 30 days: 1,240 exports, 3 support tickets about Excel encoding
```

## Safety and legacy safeguards

- Never pushes, merges, deploys, or triggers CI/CD. You review the diff and ship it yourself.
- No new dependencies, upgrades, or language/runtime version changes without your explicit approval; `npm audit fix` and similar are blocked.
- Reuse first: existing packages at their installed versions → standard library → plain code → only then a new package.
- Version-aware code: new code may use any syntax the project's language version allows (e.g. `?.` / `??`), never newer.
- No drive-by refactoring of untouched code.
- A build/test baseline is recorded before changes; only new failures block. Pre-existing failures and legacy debt are reported separately.
- Optional characterization tests pin down current behavior before risky changes.
- Mockups are self-contained HTML in `.12sp/mockups/`, never project code, and use placeholder art only (no copyrighted characters or logos).

## Models

| Agent | Model |
|---|---|
| requirements-reviewer, solution-architect, developer, security-reviewer, compliance-reviewer, acceptance-reviewer, migration-planner, post-launch-reviewer | opus |
| ui-designer, code-quality-reviewer, test-writer | sonnet |
| user-manual-writer | haiku |

Aliases always use the newest model in each tier; change the `model:` line in any agent file to adjust. Running the main session on Sonnet (`/model sonnet`) is usually enough, since it mainly coordinates. Don't set `CLAUDE_CODE_SUBAGENT_MODEL`, as it can override these choices.

## Files

```
.claude/12sp/rules.md   hard rules, preflight, handoff, status format (edit once, applies to all commands)
.12sp/                  created in the project at run time:
  ui.md, mockups/       from /12sp-ui
  plan.md               from /12sp-plan
  status.md             progress of the current run
  review.md             from /12sp-review
  post-launch.md        from /12sp-post
  archive/              finished runs
```

The architect agent is named `solution-architect` so it doesn't clash with pstack's `architect` skill.
