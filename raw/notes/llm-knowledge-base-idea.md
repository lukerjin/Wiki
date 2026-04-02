# LLM Knowledge Bases — The Idea

Source: Andrej Karpathy-style workflow, widely discussed in AI communities (2025-2026).

## Core Concept

Using LLMs to build and maintain personal knowledge bases. Instead of manually curating notes, you feed raw sources (articles, papers, repos) to an LLM and have it "compile" a structured, interlinked wiki.

## Key Workflow Steps

1. **Data Ingest**: Index source documents into a `raw/` directory
2. **Compilation**: LLM incrementally reads sources and builds a wiki (collection of .md files)
3. **IDE/Viewer**: Use Obsidian as the frontend to view the compiled wiki
4. **Q&A**: Ask the LLM complex questions against the wiki — it researches and synthesizes answers
5. **Output**: Generate slides (Marp), visualizations (matplotlib), reports — all viewable in Obsidian
6. **Linting**: Run health checks to find inconsistencies, suggest new articles, impute missing data
7. **Feedback Loop**: Query outputs get filed back into the wiki, compounding knowledge

## Why It Works

- At moderate scale (~100 articles, ~400K words), well-maintained indexes beat vector search / RAG
- The LLM auto-maintains index files with brief summaries of all documents
- Plain .md files are universally compatible and version-controllable
- Obsidian provides rich rendering (links, graphs, slides) with zero custom UI work

## Tools Mentioned

- **Obsidian**: Wiki viewer / IDE
- **Obsidian Web Clipper**: Convert web articles to .md
- **Marp**: Slide presentations in markdown
- **Claude Code**: The LLM agent that operates on the wiki
- **Custom CLI tools**: Search engine, data processors

## Open Questions

- How to scale beyond ~400K words? (synthetic data + finetuning?)
- Could this become a product instead of a hacky collection of scripts?
- How to handle multimedia sources (video, audio)?
