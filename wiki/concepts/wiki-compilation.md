---
title: Wiki Compilation
tags: [compilation, workflow, core-process]
created: 2026-04-02
updated: 2026-04-02
sources: [llm-knowledge-base-idea]
---

# Wiki Compilation

> The process of an LLM reading raw source materials and producing structured, interlinked wiki articles.

## Overview

Wiki compilation is the core operation of the LLM knowledge base. When new raw sources are added, the LLM reads them, extracts key concepts, creates summaries, writes or updates concept articles, and maintains all cross-references and indexes. This is analogous to a compiler transforming source code into an executable — raw data is transformed into navigable knowledge.

## Key Points

- Compilation is incremental — only new/changed sources are processed
- Each source produces a summary + updates to relevant concept articles
- Indexes are updated atomically after each compilation pass
- The process is idempotent — running it twice on the same source produces the same result

## The Compilation Pipeline

1. **Scan**: Check `raw/` for files not yet in the master index
2. **Summarize**: Create structured summary for each new source
3. **Extract**: Identify key concepts mentioned in the source
4. **Write/Update**: Create new concept articles or enhance existing ones
5. **Link**: Add backlinks, cross-references, and update related articles
6. **Index**: Update master index and topic map

## Related Concepts

- [[llm-knowledge-base]] — the system this process serves
- [[index-over-rag]] — how indexes enable efficient navigation post-compilation

## Sources

- [[summaries/llm-knowledge-base-idea]] — describes the compilation workflow

