# Knowledge-

A voice-driven personal knowledge management system built on Zettelkasten principles. Voice notes, meeting transcripts, and email threads are processed through Claude into enhanced notes and atomic insights, connected through a consistent tag library into a searchable knowledge graph.

## How It Works

```
Raw Input (voice, meeting, email)
    → Transcription (if audio)
    → Claude Processing
    → Enhanced Note + Atomic Insights
    → User Review/Edit
    → SQLite Index (tags, embeddings, FTS)
    → Knowledge Graph (connections via shared tags)
```

1. **Capture** — Voice notes, Fathom transcripts, Wispr dictations, emails drop into `/raw/`
2. **Process** — Claude produces an enhanced note (coherent, tagged) + atomic insight cards
3. **Review** — User reviews and edits insights during processing (or revisits later)
4. **Connect** — Tags create links between notes and insights, building the graph automatically
5. **Reflect** — Morning surfacing, evening integration, weekly synthesis

## Core Insight

**Filing is the bottleneck.** Most knowledge systems fail at organization, not capture. This system replaces folders with tags — each note gets 5-15 tags creating multiple retrieval pathways. A note about a client meeting connects to a note about personal boundaries connects to a podcast reflection, because they share tags.

Tags aren't categories. They're hyperlinks. The tag library enforces consistency, and new tags emerge organically only when content covers genuinely new territory.

## Two-Tier Note Architecture

```
/notes/
├── enhanced/     ← Full processed notes (parent notes)
└── insights/     ← Atomic idea notes (Zettelkasten-style)
```

**Enhanced notes** are coherent, structured versions of raw inputs — preserving authentic voice while improving clarity. One per raw input.

**Insight notes** are atomic ideas extracted from enhanced notes:
- One idea per note
- Written in full sentences, understandable without the source
- Framed as a claim, observation, or question — not a summary
- Linkable back to the parent enhanced note
- Independently taggable (5-15 tags each)

This two-tier structure solves the "source-bound" problem: enhanced notes organize inputs, while insights are abstracted ideas that freely combine across sources.

## Storage: Hybrid Architecture

- **Markdown files are the source of truth** — human-readable, portable, Obsidian-compatible
- **SQLite is a derived index** — tags, relationships, embeddings, full-text search (FTS5)
- The index rebuilds from files. Data is never locked inside a database.
- Insight notes use YAML frontmatter for structured metadata and links

## Daily Workflow

| Time | Activity | System Support |
|------|----------|----------------|
| Morning (5:20am) | Prime the day | Surface notes relevant to today's calendar + recent themes |
| Throughout day | Capture | Drop files to `/raw/`, record in-app, auto-ingest from Fathom/Wispr |
| As processed | Review | Read enhanced notes + insights, edit, approve |
| Evening | Reflect | Reflection based on day's activities and connections |
| Weekly | Synthesize | Synthesis across the week's notes, tag health check |
| Monthly | Maintain | Connection discovery, tag consolidation |

## Integrations (Planned)

- **Fathom** — Meeting transcripts via API/webhook
- **Wispr Flow** — Voice dictation transcripts
- **Google Calendar** — Morning briefing pulls today's schedule to surface relevant notes
- **Content generation** (future) — LinkedIn, blog, Twitter posts from recombinant insights

## Build Order

### Phase 1 — API Backend
- FastAPI application with Claude API integration
- SQLite storage layer (notes index, FTS5, embeddings)
- Processing pipeline: raw transcript → enhanced note + insights
- Tag library management (CRUD, validation, merge, deprecation)
- Markdown file read/write with YAML frontmatter
- Search (full-text, by tag, by date, semantic similarity)

### Phase 2 — Web UI
- Morning briefing view (today's calendar context + relevant notes)
- Note browser (enhanced notes + insights, filterable by tag/date)
- Processing review interface (approve/edit insights during processing)
- Evening reflection view
- Knowledge graph visualization (tag clusters, connections)
- Tag library management UI

### Phase 3 — File Watcher & Automation
- Watch `/raw/` for new files, auto-trigger processing
- Fathom API integration (auto-ingest meeting transcripts)
- Wispr Flow integration (auto-ingest voice dictations)
- Google Calendar integration (context for morning briefing)
- Weekly synthesis auto-generation

### Phase 4 — Mobile App
- Voice capture (record directly in app)
- Morning briefing (read during REFLECT block)
- Quick review (skim new notes and insights)
- Full note reading and editing
- PWA or React Native — TBD

### Future — Content Generation
- Select insights → generate platform-specific posts
- Weekly themes → draft posts
- Content links back to source insights in knowledge graph
- Platform-specific voice/style profiles

## Tech Stack

- **Backend**: Python / FastAPI
- **Database**: SQLite (derived index — FTS5, embeddings, relationships)
- **AI**: Claude API (processing, insight extraction, synthesis, content generation)
- **File format**: Markdown with YAML frontmatter
- **Frontend**: Web (framework TBD), then mobile
- **File watching**: watchdog (Python)

## Project Structure

```
Knowledge-/
├── CLAUDE.md              # Project context for Claude Code sessions
├── README.md              # This file
├── docs/
│   └── architecture.md    # Detailed architecture decisions
├── system/                # System docs (from Co-work prototype)
│   ├── system-prompt.md   # Processing templates
│   ├── tag-library.md     # 111 tags, 14 categories
│   ├── tag-maintenance.md # Maintenance prompts
│   ├── daily-practice.md  # Daily practice prompts
│   ├── processed-files.md # Manifest of 58 processed files
│   └── name-corrections.md# Transcription error corrections
├── notes/
│   ├── enhanced/          # Full processed notes
│   └── insights/          # Atomic idea notes
├── raw/                   # Unprocessed inputs
├── api/                   # FastAPI endpoints
├── core/                  # Processing pipeline, search, graph
├── models/                # Data models / schemas
├── storage/               # SQLite index layer
├── web/                   # Frontend
└── tests/
```

## Processing Principles

1. Preserve authentic voice — coherent and complete, not sanitized
2. Apply existing tags first — always check the tag library before creating new ones
3. Create new tags sparingly — only for genuinely new territory
4. Include source references — when responding to specific content
5. Extract atomic insights — standalone ideas that can freely recombine
6. Suggest connections — flag related notes or themes when patterns emerge

## What Good Output Looks Like

A well-processed enhanced note:
- Reads clearly while maintaining original voice and thinking style
- Includes 5-15 tags creating multiple retrieval pathways
- References sources when responding to specific content
- Connects to the existing knowledge graph through shared tags

A well-extracted insight:
- One idea, fully expressed in 2-5 sentences
- Understandable without reading the parent note
- Framed as a claim, observation, or question
- Tagged independently for maximum graph connectivity
- Links back to parent note for provenance
