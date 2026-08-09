---
name: lit-review
description: Systematic literature review, related-work scans, and fact-checking for academic papers. Searches for papers, verifies every source exists, synthesizes findings thematically with gap analysis, and outputs a verified .bib file plus annotated notes. Use when the user asks for a literature review, related work, prior work, a survey of a topic, paper comparisons, "what has been done on X", finding papers/citations for a claim, or fact-checking research claims. Trimmed adaptation of Imbad0202/deep-research.
---

# Literature Review

Find papers, verify they exist, synthesize them, and produce a verified bibliography. The `.bib` file is the single source of truth: no paper may be cited in any prose unless it has a verified entry in the `.bib` first.

## Modes

Pick based on what the user needs; state your choice in one line.

| Mode | When | Output |
|------|------|--------|
| **lit-review** (default) | Related-work section, survey of an area | Annotated bibliography + thematic synthesis + gap analysis |
| **three-way-scan** | Fast comparison of a shortlist of papers | Per-paper WHY/HOW/WHAT cards + cross-paper synthesis |
| **fact-check** | Verify specific claims or citations | Per-claim verdict with sources |

## Workflow

Copy this checklist and check off steps as you go:

```
- [ ] 1. Scope: restate the question, in/out-of-scope boundaries
- [ ] 2. Search: multiple angles, log queries used
- [ ] 3. Verify: every candidate paper confirmed to exist
- [ ] 4. Build .bib: verified entries only, run citation-checker
- [ ] 5. Synthesize: themes, contradictions, gaps
- [ ] 6. Deliver: notes cite only \cite{keys} present in the .bib
```

**1. Scope.** One paragraph: the question, what's in scope, what's out. For a paper's related-work section, organize scope around the paper's contributions (what must be differentiated against).

**2. Search.** Use several angles, not one query: by method, by problem, by dataset/benchmark, by seminal-paper citation trails ("who cites X"). Sources: arXiv, Semantic Scholar, Google Scholar, OpenReview. Log the queries you ran so the search is reproducible. Target recent work first (last 2–3 years) plus the seminal papers everyone builds on.

**3. Verify.** For each candidate: confirm title, authors, year, and venue against the actual paper page (arXiv abstract page, DOI, or publisher page — not a search-snippet). Gray zone = FAIL: if you cannot confirm a paper exists, it does not get cited or listed anywhere. Never assemble a reference from memory.

**4. Build the .bib.** Write a BibTeX entry for every verified paper, with DOI or arXiv ID whenever available. Then run the citation-checker skill's script over it:

```bash
python ~/.claude/skills/citation-checker/scripts/citation_checker.py refs.bib
```

Fix or delete anything not verified. Only after this passes may entries be cited.

**5. Synthesize.** Organize by theme, not paper-by-paper. For each theme: what the papers agree on, where they contradict (report both sides), and what remains open. End with an explicit gap analysis — the gaps are usually the point for Juliana's own papers. Every claim in the synthesis carries a `\cite{key}` from the .bib.

**6. Deliver.** Annotated bibliography (2–4 sentences per paper: what it did, key result, relevance to the question) + synthesis + the verified `.bib`. Prose follows the scientific-writing skill.

## Three-way-scan format

One card per paper, then a cross-paper summary:

```markdown
## <title> (<first author> <year>, <venue>) — \cite{key}
- WHY: problem addressed and why it matters
- HOW: method / technical route
- WHAT: findings and what's left unresolved
```

Cross-paper: shared WHY, divergent HOWs, strongest WHAT, the unresolved gap.

## Fact-check format

Per claim: **verdict** (supported / contradicted / unverifiable) + the verified source(s) + one sentence of evidence. Unverifiable claims are reported as such, never softened into "likely true".

## Anti-patterns

- **Vibe citing**: blending elements of 2–3 real papers into one reference. Every reference is verified independently against its own paper page.
- **Cherry-picking**: citing the one supportive study and ignoring contradicting ones. Report the full evidence landscape.
- **Search-snippet citation**: citing based on a search-result summary without opening the paper. Read at least the abstract; say so if you read only the abstract.
- **Tier inflation**: blog posts and tweets are not peer-reviewed papers; label non-peer-reviewed sources (preprint, blog, workshop) explicitly.
- **Citing outside the .bib**: if a paper isn't in the verified .bib, it doesn't appear in prose. Add and verify it first.
