---
name: orient
description: Orients a new session in the current project by reading its state docs, recent git history, and memory before doing any work. Use at the start of a session, when invoked as /orient, when the user says "catch up", "get up to speed", "where were we", or "read the docs first", or before starting substantive work in a project whose current state is not yet known this session.
---

# Session Orientation

Read the project's actual state before acting. Do this in one pass, then summarize.

1. **Project docs.** In the working directory, read whichever of these exist: `PLAN.md`, `PROGRESS.md`, `README.md`, design docs (`*design_doc*.md`), `SESSION_LOG.md`. PROGRESS-style docs matter most: read the newest entries fully, skim the rest.
2. **Git state.** `git log --oneline -15` and `git status --short` — what changed recently, what's uncommitted, what branch.
3. **Memory.** Recalled memories arrive automatically; verify anything load-bearing (paths, quotas, standing decisions) against the docs just read — docs win over memory when they disagree.
4. **Summarize back** in a few sentences: where the project stands, what the last session did, what appears to be next, and anything surprising (uncommitted changes, failed runs, stale plans). Ask nothing unless genuinely blocked.

Do not start editing files or launching jobs during orientation. Orientation ends with the summary; the user's request drives what happens next.
