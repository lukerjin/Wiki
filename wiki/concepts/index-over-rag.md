---
title: Index Over RAG
tags: [rag, indexing, scaling, architecture]
created: 2026-04-02
updated: 2026-04-02
sources: [llm-knowledge-base-idea]
---

# Index Over RAG

> At moderate scale, auto-maintained plain-text indexes outperform vector search (RAG) for LLM-driven knowledge bases.

## Overview

A key insight of the LLM knowledge base approach is that you don't need Retrieval Augmented Generation (RAG) with vector databases at moderate scale. Instead, the LLM maintains concise index files — a master index with one-line summaries of every article, and a topic map showing the hierarchy. These indexes are small enough to fit entirely in the LLM's context window, allowing it to efficiently locate and read any relevant article.

## Key Points

- A master index of ~500 articles with one-line summaries fits in ~10K tokens
- The LLM can scan the index, identify relevant articles, then read them directly
- No embedding pipeline, vector database, or retrieval infrastructure needed
- Index maintenance is a natural byproduct of the compilation process
- RAG becomes necessary only at very large scale (thousands of articles)

## When RAG Becomes Necessary

| Scale | Articles | Words | Strategy |
|-------|----------|-------|----------|
| Small | < 50 | < 100K | Read everything directly |
| Medium | 50-500 | 100K-1M | Index-based navigation |
| Large | 500-5000 | 1M-10M | Index + search tools |
| Very Large | 5000+ | 10M+ | RAG / finetuning needed |

## Related Concepts

- [[llm-knowledge-base]] — the system this strategy serves
- [[wiki-compilation]] — the process that maintains the indexes

## Sources

- [[summaries/llm-knowledge-base-idea]] — discusses why indexes beat RAG at ~400K words

