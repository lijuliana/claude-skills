---
name: close
description: End-of-session sweep that persists what the session learned before context is lost. Use when invoked as /close, or when the user says they're wrapping up, done for the day, or asks to save/log the session's state.
---

# Session Close

Three phases; report a one-line tally at the end.

## 1. Retrospective

Scan this session for things worth keeping — only what a future session would need:
- **Decisions** made (and why) that aren't yet written anywhere
- **Corrections/preferences** the user expressed about how to work
- **Open tasks** left unfinished, with enough context to resume
- **References** discovered (URLs, dashboards, repos)

Write each to the memory directory using the existing frontmatter format (`type: user | feedback | project | reference`), updating existing files over creating duplicates, and index new files in `MEMORY.md`. Skip anything the repo already records.

## 2. Housekeeping

- If in a git repo: show `git status --short` and `git diff --stat HEAD`. If there are meaningful uncommitted changes, propose a commit message and ask before committing.
- If the repo has a `PROGRESS.md` or `SESSION_LOG.md`, prepend a dated entry: 1–2 sentences on what the session did and a pointer to the main artifact. Create `SESSION_LOG.md` at the repo root only if neither exists and the session produced real work.

## 3. Close out

- Print a session-name suggestion: `[YYYY-MM-DD] <topic> - <what was done>`.
- Print the tally: `<N> memory updates · <N> commits · log updated? yes/no`.
