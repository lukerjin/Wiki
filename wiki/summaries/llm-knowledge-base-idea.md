---
title: "LLM Knowledge Bases — The Idea"
source: raw/notes/llm-knowledge-base-idea.md
author: Community synthesis (Karpathy-style workflow)
date: 2025-01-01
type: note
tags: [llm, knowledge-base, obsidian, wiki, workflow]
indexed: 2026-04-02
---

# Summary: LLM Knowledge Bases — The Idea

## Key Points

- LLMs can serve as both the builder and query engine for personal knowledge bases
- The workflow is: ingest raw sources → LLM compiles structured wiki → query via LLM → outputs feed back in
- Plain-text indexes (master index + topic map) provide sufficient navigation at moderate scale (~400K words)
- Obsidian serves as a zero-config rich viewer for the compiled wiki
- Health checks and linting by the LLM improve data integrity over time

## Summary

The core idea is to use an LLM agent (like Claude Code) to manage the entire lifecycle of a personal knowledge base. Raw sources — articles, papers, notes, images — are collected into a directory. The LLM then "compiles" these into a structured wiki of interlinked markdown files, extracting concepts, creating summaries, and maintaining indexes. The same LLM is then used for Q&A against the wiki, generating outputs (slides, visualizations, reports), and running maintenance tasks (finding inconsistencies, suggesting new articles). All outputs are viewable in Obsidian. The key insight is that at moderate scale, auto-maintained plain-text indexes beat RAG/vector search approaches.

## Concepts Extracted

- [[concepts/llm-knowledge-base]]
- [[concepts/wiki-compilation]]
- [[concepts/index-over-rag]]
- [[concepts/obsidian-as-viewer]]
- [[concepts/knowledge-feedback-loop]]

## Notable Quotes / Data

> "You rarely ever write or edit the wiki manually, it's the domain of the LLM."

> "At ~100 articles and ~400K words, well-maintained indexes beat vector search."

