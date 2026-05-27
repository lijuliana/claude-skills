---
name: scientific-writing
description: Juliana's scientific writing and citation rules. Use whenever writing or editing scientific prose — papers, abstracts, related-work sections, progress reports, design docs headed for a paper, figure captions, rebuttals — whenever making figures for a paper, and whenever adding citations to prose (citations must come from a verified .bib). Triggers on drafting, editing, condensing, or reviewing any text headed for a paper or research write-up.
---

# Scientific Writing Rules

Apply these rules to every piece of scientific writing (papers, abstracts, sections, captions, rebuttals, research summaries) and to paper figures.

## Storyline

- Lead with one clear storyline. State the problem, the gap, the contribution, the strongest evidence, and the consequence, in that order. Put surprising points first, in simple terms.
- Use concrete results early. The strongest quantitative result or theorem goes in the abstract, introduction, and results section; the reader must not have to search for it.
- Keep the structure tight and concise. The reader should be able to follow a compelling line of reasoning without effort.

## Sentence-level style

- Declarative sentences stating facts in the order a reader needs them. No editorializing, no narrative voice, no rhetorical questions, no fragments.
- Remove LLM-style phrasing. Banned outright:
  - em dashes
  - "not X but Y" constructions
  - "real" / "genuine" as empty emphasis
  - "smoking gun"
  - rhetorical questions
  - canned contrasts
  - grand closing slogans
- Delete any clause not essential to understanding. Specifically banned patterns:
  - appositive self-labels ("and this is our central prediction", "recorded before starting")
  - meta-commentary about the document itself ("the following are fixed now so no outcome can be re-narrated", "reporting commitment" sections, honesty notes)
  - justification-of-process asides
- No unnecessary adjectives/adverbs: sharply, striking, remarkably, notably, crucially, real, clean, genuinely, decisively.
- No jargon or lab shorthand: think block, potency check, battery, cell, harness, baseline, arm, norm, prompting condition, "no-reasoning baseline". Use plain words (reasoning section, experiment, setup) or define the term at first use in one sentence.
- Every phrase must make sense at first glance to a lay reader. If a phrase needs prior context ("serves as storage" — storage of what?), spell it out ("holds intermediate results the model can read back").
- No coined terms or metaphors carried across sentences ("the page", "ladder", "sledgehammer", "frontier" as branding).
- No triadic parallel constructions or quippy compressed topic sentences ("The trade-off replicates.").
- Condense: multi-sentence definitions into one where possible; paragraphs with many numbers down to the few that carry the finding.

## Figures

- Figures are part of the argument. Professional layouts, readable labels, appropriate sizing, accessible colors, consistent formatting, and only enough detail to make the main pattern immediately clear.
- Reference figures from strong past NeurIPS papers as the visual bar (sizing, colors, fonts, layouts). A figure must be understandable at first glance.

## Citations

- The `.bib` file is the single source of truth. Cite only `\cite{keys}` that exist in the verified `.bib`; never write a citation from memory.
- To cite a paper not yet in the `.bib`: add a BibTeX entry with DOI/arXiv ID, verify it with the citation-checker skill's script, then cite it.
- If a needed reference cannot be verified, leave a `% TODO: needs verified ref` comment instead of a citation.

## The test

If a sentence couldn't be understood at first glance, it won't make sense to the reader. Rewrite it.
