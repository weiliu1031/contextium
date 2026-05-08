---
name: contextium
description: Use when the user asks to close, archive, summarize, or commit the current Contextium session, including commands like $contextium, contextium summary, session end, wrap up, or close this out.
argument-hint: "[--no-push] [--new-commit]"
---

# contextium: Archive Contextium Session

Close the current Contextium session by writing a journal entry, maintaining one daily Contextium commit, and pushing it.

## Workflow

1. Work from the Contextium repo root.
2. Load:
   - the active agent instruction file (`AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, etc.)
   - `preferences/templates/journal_template.md`
   - today's `journal/YYYY-MM-DD.md` if it exists
3. Summarize only the current session:
   - user request and outcome
   - files changed
   - decisions made
   - follow-ups or open questions
4. Create or update `journal/YYYY-MM-DD.md`.
   - For a light session, add a short heading and concise bullets.
   - Do not copy source code, build artifacts, logs, or secrets into the journal.
5. Check `git status --porcelain`.
6. Stage only the specific journal/context files changed in this archive step.
   - Never use `git add -A`.
7. Maintain one Contextium commit per day:
   - Use subject `doc: update contextium journal`.
   - Commit body MUST be a bullet list of today's archived items.
   - Each bullet should state one concrete session outcome.
8. Decide whether to create or amend:
   - If `$ARGUMENTS` contains `--new-commit`, create a new commit.
   - Otherwise inspect `git log -1 --format='%ad%x00%s' --date=short`.
   - If HEAD author date is today's date and subject is exactly
     `doc: update contextium journal`, amend HEAD.
   - If HEAD is anything else, create a new commit.
9. Commit command:
   - New commit: `git commit -s -F /tmp/contextium-commit-msg`.
   - Same-day amend: `git commit --amend -s -F /tmp/contextium-commit-msg`.
   - For amend, preserve existing body bullets and append the new session item.
10. Unless `$ARGUMENTS` contains `--no-push`, push:
   - New commit: `git push origin main`.
   - Amended commit: `git push --force-with-lease origin main`.

## Safety Rules

- Do not stage unrelated user edits.
- Do not include credentials, tokens, or private command output in the final response.
- If the working tree already contains unrelated changes, leave them unstaged and mention them briefly.
- Do not amend a non-Contextium commit.
- If commit or push fails, report the exact blocker and leave the staged state clear when possible.

## Final Response

Report:
- journal file updated
- commit hash, if created
- push status
- any unrelated changes left untouched
