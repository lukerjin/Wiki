# CLAUDE.md — Wiki Knowledge Base Instructions

You are the maintainer of this LLM-powered knowledge base. This file defines how you should operate on the wiki.

## Directory Structure

- `raw/` — Source materials added by the user. You READ from here, never delete.
- `wiki/` — The compiled knowledge base. You WRITE and maintain this.
- `wiki/_index/` — Index files you maintain for fast navigation.
- `wiki/concepts/` — One article per key concept.
- `wiki/summaries/` — One summary per source document.
- `wiki/connections/` — Cross-cutting analysis linking multiple concepts.
- `wiki/outputs/` — Generated outputs (slides, visualizations, analysis results).
- `tools/` — CLI tools for wiki operations.

## Core Operations

### 1. Compile (Ingest new raw sources)

When asked to "compile the wiki" or "process new sources":

1. Read `wiki/_index/master-index.md` to know what's already indexed
2. Scan `raw/` for files NOT listed in the master index
3. For each new source:
   a. Read the source file thoroughly
   b. Create a summary in `wiki/summaries/<source-name>.md` with:
      - Source metadata (title, author, date, URL if available)
      - Key points (bullet list)
      - One-paragraph summary
      - Tags/topics
   c. Identify key concepts mentioned
   d. For each concept, either create or update `wiki/concepts/<concept>.md`
   e. Look for connections to existing concepts → update `wiki/connections/` if meaningful
4. Update `wiki/_index/master-index.md` with new entries
5. Update `wiki/_index/topic-map.md` with any new topics
6. Update `wiki/_index/recent-changes.md` with what was done

### 2. Query (Answer questions against the wiki)

When asked a question about the wiki's contents:

1. Read `wiki/_index/master-index.md` and `wiki/_index/topic-map.md` first
2. Identify which articles are relevant to the question
3. Read those specific articles
4. Synthesize an answer with citations (links to wiki articles)
5. If asked to save the output, write it to `wiki/outputs/`

### 3. Health Check (Lint and enhance)

When asked to run a health check:

1. Verify all links in wiki articles resolve to existing files
2. Check that every source in `raw/` has a corresponding summary
3. Find concepts mentioned but not yet having their own article
4. Identify potential inconsistencies or contradictions
5. Suggest new connections or article candidates
6. Write results to `wiki/outputs/health-check-<date>.md`

### 4. Output Generation

When asked to generate outputs (slides, visualizations, etc.):

1. Research the topic in the wiki using the index files
2. Generate the requested format:
   - **Marp slides**: Write `.md` with Marp frontmatter to `wiki/outputs/`
   - **Mermaid diagrams**: Include in markdown with ```mermaid blocks
   - **Analysis reports**: Write detailed `.md` to `wiki/outputs/`
3. Always cite sources from the wiki with `[[wikilinks]]`

## Formatting Standards

### Wiki Article Template

```markdown
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

---
*Last updated: YYYY-MM-DD*
*Tags: #tag1 #tag2 #tag3*
```

### Summary Template

```markdown
# Summary: <Source Title>

| Field | Value |
|-------|-------|
| Source | <filename or URL> |
| Author | <author> |
| Date | <date> |
| Type | article / paper / note / dataset |

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

---
*Indexed: YYYY-MM-DD*
*Tags: #tag1 #tag2 #tag3*
```

### Linking Conventions

- Use `[[wikilinks]]` format for internal links (Obsidian compatible)
- Use relative paths from wiki root: `[[concepts/machine-learning]]`
- Always add backlinks when creating new connections

### Index File Format

**master-index.md**: One line per article — `- [[path/to/article]] — one-line summary`

**topic-map.md**: Nested list hierarchy of topics with article links under each.

## Important Rules

1. **Never delete raw sources** — only read from `raw/`
2. **Always update indexes** after any wiki change
3. **Use today's date** when recording changes
4. **Preserve existing content** — enhance, don't overwrite, unless correcting errors
5. **Cite sources** — every claim in a concept article should trace back to a summary
6. **Keep indexes concise** — they must fit in context for efficient navigation
