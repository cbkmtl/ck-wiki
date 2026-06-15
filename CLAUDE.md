# CLAIRE's Wiki — Schema & Workflows

This is a personal knowledge base maintained by an LLM (Claude Code). The human curates sources and directs analysis. The LLM writes and maintains all wiki files.

---

## Directory Layout

```
raw/              ← immutable source documents (never edit these)
  articles/       ← web articles, essays, clipped pages
  books/          ← book files, chapter notes, highlights
  notes/          ← personal notes, journal entries, voice memos
  assets/         ← images referenced by raw sources

wiki/             ← LLM-maintained knowledge base
  sources/        ← one summary page per source ingested
  concepts/       ← idea, framework, or theme pages
  entities/       ← person, place, organization, book pages
  topics/         ← topic overview pages (synthesis across sources)
  index.md        ← catalog of all wiki pages
  log.md          ← append-only ingest/query/lint history
  overview.md     ← high-level synthesis of the whole wiki
```

---

## Page Conventions

All wiki pages use this frontmatter:

```yaml
---
title: Page Title
type: source | concept | entity | topic
tags: [tag1, tag2]
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: N        # number of sources that touch this page
---
```

Cross-link liberally using `[[Page Title]]` (Obsidian wikilinks). Every page should link outward to related pages. Every page should be reachable via inbound links from at least one other page.

---

## Workflows

### Ingest

Triggered when the human adds a source to `raw/` and asks me to process it.

1. Read the source file(s).
2. **Discuss with the human first** — share key takeaways, ask what to emphasize, confirm framing before writing anything.
3. Write a summary page in `wiki/sources/` named after the source.
4. Update or create concept, entity, and topic pages touched by this source.
5. Update `wiki/index.md` with the new source page and any new wiki pages.
6. Append an entry to `wiki/log.md`.
7. Briefly update `wiki/overview.md` if the source shifts the big picture.

**Never ingest without first discussing with the human.**

### Query

Triggered when the human asks a question.

1. Read `wiki/index.md` to find relevant pages.
2. Read those pages in full.
3. Synthesize an answer with citations to wiki pages (and source pages where relevant).
4. If the answer is worth keeping, offer to file it as a new wiki page (in `wiki/topics/` or `wiki/concepts/`).

### Lint

Triggered when the human asks for a health check.

Check for:
- Contradictions between pages
- Stale claims superseded by newer sources
- Orphan pages (no inbound links)
- Concepts mentioned but lacking their own page
- Missing cross-references
- Data gaps worth filling

Report findings and suggest next actions.

---

## Log Format

Each log entry starts with:

```
## [YYYY-MM-DD] <operation> | <title>
```

Operations: `ingest`, `query`, `lint`, `create`

This makes entries grep-able: `grep "^## \[" wiki/log.md | tail -10`

---

## Style Notes

- Write wiki pages as reference material, not chat. Dense, linked, no filler.
- Flag contradictions explicitly with a `> ⚠️ Contradiction:` blockquote.
- When a newer source updates a claim, note it inline: *(updated [date] — see [[Source]])*
- Keep source summary pages factual. Keep concept and topic pages analytical.
- The human's voice and framing should show through — ask about their take when ingesting.
