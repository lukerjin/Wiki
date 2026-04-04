# Wiki — LLM-Powered Personal Knowledge Base

An LLM-powered personal knowledge base system built on Claude Code + Obsidian.

## What is this?

A system where you collect raw source materials (articles, papers, notes, images), and an LLM (Claude Code) **compiles** them into a structured, interlinked wiki. You rarely edit the wiki manually — the LLM writes, maintains, and enhances it. You read and query it in Obsidian.

## Architecture

```
raw/                  # Source materials (articles, papers, notes, images)
  ├── articles/       # Web articles (.md via Obsidian Web Clipper)
  ├── papers/         # Research papers and PDFs
  ├── notes/          # Your own quick notes and observations
  └── images/         # Referenced images

wiki/                 # LLM-compiled knowledge base (DO NOT EDIT MANUALLY)
  ├── _index/         # Auto-maintained indexes and log
  │   ├── index.md           # Catalog of all articles (LLM reads this first)
  │   ├── topic-map.md       # Topic hierarchy and relationships
  │   └── log.md             # Append-only chronological operation log
  ├── concepts/       # Concept articles (one per key concept)
  ├── summaries/      # Source document summaries
  ├── connections/    # Cross-cutting analysis and connections
  └── outputs/        # Query results, slides, visualizations

tools/                # CLI tools for wiki operations
scripts/              # Helper scripts
CLAUDE.md             # Instructions for Claude Code (the "brain")
```

## Quick Start

### 1. Setup

```bash
# Clone this repo
git clone <repo-url> && cd Wiki

# Open in Obsidian (File > Open Vault > select this directory)
# Install recommended plugins: Obsidian Web Clipper, Marp Slides
```

### 2. Ingest Data

Drop source files into `raw/`:
- **Web articles**: Use Obsidian Web Clipper to save as .md into `raw/articles/`
- **Papers/PDFs**: Copy into `raw/papers/`
- **Quick notes**: Write into `raw/notes/`
- **Images**: Save into `raw/images/`

### 3. Compile Wiki

Open Claude Code in this directory and run:

```
Compile the wiki — process any new files in raw/ that haven't been indexed yet.
```

Claude Code will:
1. Scan `raw/` for new/changed source files
2. Create summaries in `wiki/summaries/`
3. Extract concepts and create/update articles in `wiki/concepts/`
4. Update all indexes in `wiki/_index/`
5. Add backlinks and cross-references

### 4. Query the Wiki

Ask Claude Code questions against your knowledge base:

```
Based on the wiki, what are the key differences between X and Y?
```
```
Create a Marp slide deck summarizing the main findings about X.
```
```
Find inconsistencies or gaps in the wiki's coverage of topic X.
```

### 5. Maintain & Enhance

```
Run a health check on the wiki — find broken links, missing summaries, and inconsistencies.
```
```
Suggest 5 new article candidates based on connections between existing concepts.
```

## How It Works

### The Compilation Loop

```
raw/ sources  →  LLM reads & summarizes  →  wiki/summaries/
                 LLM extracts concepts   →  wiki/concepts/
                 LLM finds connections   →  wiki/connections/
                 LLM updates indexes     →  wiki/_index/
```

### Index Strategy

Instead of fancy RAG, the system maintains plain-text indexes:
- **index.md**: Every article with a one-line summary → lets the LLM quickly find relevant docs
- **topic-map.md**: Hierarchical topic tree → lets the LLM navigate by domain
- **log.md**: Append-only operation log → parseable with `grep "^## \[" log.md`
- These files are small enough to fit in context, enabling efficient lookup at scale

### Output Formats

| Format | Use Case | View In |
|--------|----------|---------|
| Markdown | Articles, summaries, analysis | Obsidian |
| Marp slides | Presentations, overviews | Obsidian (Marp plugin) |
| Matplotlib/SVG | Data visualizations, diagrams | Obsidian |
| Mermaid | Flowcharts, concept maps | Obsidian (native support) |
| Canvas | Spatial layouts, mind maps | Obsidian (native .canvas) |

## Obsidian Setup Tips

### Recommended Plugins

- **Obsidian Web Clipper** — browser extension to save web articles as .md into `raw/articles/`
- **Marp Slides** — render LLM-generated slide decks
- **Dataview** — query wiki pages by YAML frontmatter (e.g. list all articles tagged `#ai` sorted by date)

### Download Images Locally

In Obsidian: Settings → Files and Links → set "Attachment folder path" to `raw/images/`. Then in Settings → Hotkeys, search "Download" and bind "Download attachments for current file" to a hotkey (e.g. `Ctrl+Shift+D`). After clipping a web article, hit the hotkey to download all images locally so the LLM can reference them.

### Graph View

Use Obsidian's built-in graph view to see the shape of your wiki — which concepts are hubs, which are orphans, how topics cluster.

## Key Principles

1. **LLM-first editing** — The wiki is the LLM's domain. You add raw sources; the LLM compiles the wiki.
2. **Plain text all the way** — Everything is .md files in directories. No databases, no special formats.
3. **Indexes over RAG** — At ~100 articles / ~400K words, well-maintained indexes beat vector search.
4. **Outputs feed back in** — Query results get filed back into the wiki, compounding knowledge.
5. **Obsidian as viewer** — Rich rendering without any custom UI needed.

## License

See [LICENSE](LICENSE).
