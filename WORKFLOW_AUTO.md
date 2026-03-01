# WORKFLOW_AUTO.md

> Auto-workflow checklist (created because file was missing after compaction).

## Startup
- Read SOUL.md, USER.md
- Read memory/YYYY-MM-DD.md (today + yesterday)
- In main session (direct chat), read MEMORY.md

## When working on code/files
- Prefer minimal, reversible edits; keep a stable “golden master” template and work on copies.
- After changes: run a quick sanity check (open in browser / check console for errors).
- Use `TEST_CHECKLIST.md` before any “lock/release”.
- Record important decisions/milestones in daily memory and distill into MEMORY.md when stable.
- For new web/game projects: follow `WEB_GAME_WORKFLOW.md` + fill `PRD_ONEPAGER.md` + maintain `BACKLOG.md`.

## OpenClaw ops notes (keep this section up to date)
### Cron: avoid duplicate delivery
- If you create cron jobs with `--session isolated` and `--announce`, the job will *directly deliver* to the chat target.
- If the main session also forwards the cron result, users may see duplicates.
- Preferred pattern for human-facing reminders in this workspace:
  - Use `--session main` + `--system-event` (no direct delivery), then the main session sends exactly one message.
- Useful commands:
  - `openclaw cron list --json`
  - `openclaw cron edit <id> ...`
  - `openclaw cron rm <id>`
- Doc: `docs/cli/cron.md` (notes: isolated jobs default announce; use `--no-deliver` to keep internal)

## When unsure
- Ask for the exact file/link/screenshot and reproduce before proposing large refactors.
