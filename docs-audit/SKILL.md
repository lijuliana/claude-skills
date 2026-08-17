---
name: docs-audit
description: Audits a repo's documentation (PLAN, PROGRESS, README, design docs, CLAUDE.md) for contradictions, drift from the code, staleness, overlap, and unclear division of purpose. Use when invoked as /docs-audit, when the user asks to check/clean up/reconcile docs, says the docs are out of date or contradictory, or after a large pivot or milestone that the docs may not reflect. Reports findings first; edits or deletes only on request.
---

# Docs Consistency Audit

Audit every prose doc in the repo against each other and against the code. Findings first — do not edit or delete anything until the user approves, unless they asked for fixes up front.

## 1. Inventory

List the docs: `*.md` at the repo root and in `docs/`, plus `CLAUDE.md` and memory if present. For each, state its apparent job in a few words. Flag immediately any two docs whose jobs overlap.

Canonical division of labor (adapt to the repo's actual conventions):
- **README** — what the project is, how to install/run it. For outsiders. No status.
- **PLAN** — what will be done next and why. Forward-looking only.
- **PROGRESS** — what happened, dated, append-style. Backward-looking only.
- **Design docs** — why the design is the way it is. Stable rationale, not status.
- **CLAUDE.md** — standing instructions for the agent. No project narrative.

## 2. Cross-doc consistency

- Statements of fact that appear in more than one doc: do they agree (numbers, costs, quotas, dataset sizes, model choices, decisions)? Every disagreement is a finding; identify which doc is right using git history or code.
- Decisions and pivots: is any doc still describing an approach another doc records as abandoned?
- PLAN vs PROGRESS: are any "next steps" in PLAN already done, failed, or obsoleted per PROGRESS?

## 3. Docs vs code

Verify referenced artifacts exist and are current: file paths, script names, commands, config keys, directory layout. Run the cheap checks (`ls` the paths, `--help` nothing — just existence). A doc describing code that no longer exists is a finding; so is significant code (a new top-level module, a new pipeline) no doc mentions.

## 4. Staleness and concision

- Dated content: anything past its date (planned runs, deadlines, "as of" claims older than the repo's recent activity)?
- Dead weight: sections that no longer serve the doc's job — old status in a README, finished plans never pruned, duplicated background. Recommend the cut and where surviving content should live.
- Whole docs that are redundant or empty of unique content: recommend deletion, stating where each piece of unique content (if any) should move first.

## 5. Report

One findings list, ordered by severity: **contradiction** > **stale/wrong vs code** > **overlap/misplaced content** > **concision**. Each finding: the claim, the two locations (file + section), and the proposed fix in one sentence. End with the proposed doc-by-doc action plan (edit / merge / delete / leave alone) and wait for approval — or apply it if the user already asked for fixes, deleting only what is certainly redundant and moving unique content before any deletion.
