# CLAUDE.md — Wiki Knowledge Base Schema

You are the maintainer of this LLM-powered knowledge base. This file defines how you should operate on the wiki.

**This file is a living document.** You and the user co-evolve it over time. If a convention isn't working, a new page type is needed, or the workflow should change — propose edits to this file. The schema should grow with the wiki.

## Directory Structure

- `raw/` — Source materials added by the user. **Immutable** — you READ from here, never modify or delete.
- `wiki/` — The compiled knowledge base. You WRITE and maintain this entirely.
- `wiki/_index/` — Index and log files you maintain for fast navigation.
- `wiki/concepts/` — One article per key concept.
- `wiki/summaries/` — One summary per source document.
- `wiki/connections/` — Cross-cutting analysis linking multiple concepts.
- `wiki/outputs/` — Generated outputs (slides, visualizations, analysis results, filed queries).
- `tools/` — CLI tools for wiki operations (see Tools section below).

## Core Operations

### 1. Ingest (Process new raw sources)

When asked to "compile the wiki", "ingest", or "process new sources":

1. Read `wiki/_index/index.md` to know what's already indexed
2. Scan `raw/` for files NOT listed in the index
3. For each new source:
   a. Read the source file thoroughly
   b. Create a summary in `wiki/summaries/<source-name>.md` using the Summary Template
   c. Identify key concepts mentioned
   d. For each concept, either create or update `wiki/concepts/<concept>.md`
   e. Look for connections to existing concepts → update `wiki/connections/` if meaningful
   f. A single source may touch 10-15 wiki pages — that's normal
4. Update `wiki/_index/index.md` with new entries
5. Update `wiki/_index/topic-map.md` with any new topics
6. Append to `wiki/_index/log.md` with what was done

**Ingest modes:**
- **Interactive** (default): Ingest one source at a time. Discuss key takeaways with the user, let them guide emphasis.
- **Batch**: Ingest multiple sources at once with less supervision. Use when the user says "process all new sources" or similar.

### 2. Query (Answer questions against the wiki)

When asked a question about the wiki's contents:

1. Read `wiki/_index/index.md` and `wiki/_index/topic-map.md` first
2. Identify which articles are relevant to the question
3. Read those specific articles
4. Synthesize an answer with citations (`[[wikilinks]]` to wiki articles)
5. Output can take many forms depending on the question:
   - Markdown page or analysis report
   - Comparison table
   - Marp slide deck
   - Matplotlib/SVG chart or visualization
   - Mermaid diagram
   - Obsidian Canvas
6. **Proactively suggest filing valuable outputs back into the wiki.** Good answers — comparisons, analyses, discovered connections — should not disappear into chat history. Offer to save them to `wiki/outputs/` or as new concept/connection pages.
7. Append a `query` entry to `wiki/_index/log.md`

### 3. Lint (Health check and enhance)

When asked to run a health check or lint:

1. **Broken links**: Verify all `[[wikilinks]]` in wiki articles resolve to existing files
2. **Missing summaries**: Check that every source in `raw/` has a corresponding summary
3. **Missing concept pages**: Find concepts mentioned in articles but lacking their own page
4. **Orphan pages**: Find pages with no inbound links from other wiki pages
5. **Stale claims**: Look for claims that newer sources may have superseded or contradicted
6. **Inconsistencies**: Identify contradictions between different wiki pages
7. **Missing cross-references**: Find related concepts that should link to each other but don't
8. **Data gaps**: Suggest topics where a web search could fill missing information
9. **New article candidates**: Suggest new connections or concept pages worth creating
10. Write results to `wiki/outputs/health-check-<date>.md`
11. Append a `lint` entry to `wiki/_index/log.md`

### 4. Output Generation

When asked to generate outputs (slides, visualizations, etc.):

1. Research the topic in the wiki using the index files
2. Generate the requested format:
   - **Marp slides**: Write `.md` with Marp frontmatter to `wiki/outputs/`
   - **Mermaid diagrams**: Include in markdown with ```mermaid blocks
   - **Matplotlib charts**: Generate `.py` script + resulting image to `wiki/outputs/`
   - **Obsidian Canvas**: Write `.canvas` JSON file to `wiki/outputs/`
   - **Analysis reports**: Write detailed `.md` to `wiki/outputs/`
3. Always cite sources from the wiki with `[[wikilinks]]`
4. Append an `output` entry to `wiki/_index/log.md`

## Formatting Standards

### Wiki Article Template

```markdown
---
title: <Concept Name>
tags: [tag1, tag2, tag3]
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: [source-1, source-2]
---

# <Concept Name>

> One-line definition or summary.

## Overview

2-3 paragraph introduction to the concept.

## Key Points

- Point 1
- Point 2
- Point 3

## Details

Detailed explanation with subsections as needed.

## Related Concepts

- [[concept-a]] — how it relates
- [[concept-b]] — how it relates

## Sources

- [[summaries/source-1]] — what this source contributed
- [[summaries/source-2]] — what this source contributed
```

### Summary Template

```markdown
---
title: "<Source Title>"
source: <filename or URL>
author: <author>
date: YYYY-MM-DD
type: article | paper | note | dataset
tags: [tag1, tag2, tag3]
indexed: YYYY-MM-DD
---

# Summary: <Source Title>

## Key Points

- Point 1
- Point 2
- Point 3

## Summary

One to three paragraph summary of the source.

## Concepts Extracted

- [[concepts/concept-a]]
- [[concepts/concept-b]]

## Notable Quotes / Data

> Relevant quotes or data points.
```

### Linking Conventions

- Use `[[wikilinks]]` format for internal links (Obsidian compatible)
- Use relative paths from wiki root: `[[concepts/machine-learning]]`
- Always add backlinks when creating new connections

### Index Files

**index.md**: Content-oriented catalog. One line per article — `- [[path/to/article]] — one-line summary`. Organized by category. The LLM reads this first on every query to find relevant pages.

**topic-map.md**: Nested list hierarchy of topics with article links under each.

**log.md**: Chronological, append-only. Each entry: `## [YYYY-MM-DD] type | title` where type is `ingest`, `query`, `lint`, or `output`. Parseable with `grep "^## \[" log.md`.

## Tools

The `tools/` directory is for CLI tools that help the LLM (or user) operate on the wiki more efficiently.

At small scale (~100 pages), the index file is sufficient for navigation. As the wiki grows, consider adding:

- **Search engine**: [qmd](https://github.com/tobi/qmd) provides local BM25/vector hybrid search over markdown with CLI and MCP server support. Or build a simpler search script as needed.
- **Data processors**: Scripts for batch operations, format conversion, statistics.
- **Validators**: Automated link checking, frontmatter validation.

The LLM can help you build these tools as the need arises.

## Important Rules

1. **Never modify raw sources** — `raw/` is immutable, only read from it
2. **Always update indexes and log** after any wiki change
3. **Use today's date** when recording changes
4. **Preserve existing content** — enhance, don't overwrite, unless correcting errors
5. **Cite sources** — every claim in a concept article should trace back to a summary
6. **Keep indexes concise** — they must fit in context for efficient navigation
7. **Proactively suggest filing outputs** — valuable query results should become wiki pages
8. **Use YAML frontmatter** — enables Obsidian Dataview queries across the wiki
