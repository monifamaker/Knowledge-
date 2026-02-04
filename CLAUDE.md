# CLAUDE.md — Project Context for Claude Code

## What This Project Is

Personal knowledge management system for Monifa Porter / Parable Labs. Transforms voice notes, meeting transcripts, and email threads into a searchable knowledge graph with atomic insights.

**Current state**: Prototyped in Claude.ai chat → Claude Co-work with local markdown files. 58 processed notes, 111 tags, working system prompts. Now building it into software.

## Repository Structure

```
Knowledge-/
├── system/                  # System docs (imported from Co-work prototype)
│   ├── system-prompt.md     # Processing templates for voice/meeting/email
│   ├── tag-library.md       # 111 tags, 14 categories, cluster definitions
│   ├── tag-maintenance.md   # Maintenance prompts and schedules
│   ├── daily-practice.md    # Morning/evening/weekly practice prompts
│   ├── processed-files.md   # Manifest of 58 processed files
│   └── name-corrections.md  # Transcription error → correct spelling map
├── README.md                # Project plan (needs update)
└── docs/
    └── architecture.md      # Architecture decisions (this conversation)
```

## Architectural Decisions (from planning session 2026-02-02)

### Storage: Hybrid (Option C)
- **Markdown files are source of truth** — human-readable, portable, Obsidian-compatible
- **SQLite is a derived index** — tags, relationships, embeddings, full-text search (FTS5)
- Index rebuilds from files. Files are never locked inside a database.
- Insight notes are also markdown with YAML frontmatter for metadata/links/tags

### Note Architecture: Two-tier
```
/notes/
├── enhanced/     ← Full processed notes (parent notes)
└── insights/     ← Atomic idea notes (Zettelkasten-style)
```
- One raw input → one enhanced note → multiple insight cards
- Each insight: one idea, full sentences, standalone, linkable to parent, independently tagged
- Insight extraction is synchronous (user reviews during processing) but editable later

### Build Order
1. **API backend** (Python/FastAPI, Claude API, SQLite)
2. **Web UI** (reading, reviewing, morning/evening rituals, graph visualization)
3. **File watcher** (auto-process new files in /raw/)
4. **Mobile app** (capture + read + review — full experience)

### Tech Stack
- **Backend**: Python / FastAPI
- **Database**: SQLite (index, FTS5, embeddings) — derived from markdown files
- **AI**: Claude API for processing, insight extraction, synthesis
- **Frontend**: TBD (web first, then mobile — possibly PWA or React Native)
- **File watching**: watchdog or similar for /raw/ folder monitoring

### Key Integrations (planned)
- **Fathom**: Meeting transcripts via API/webhook (confirmed: available on free plan, webhooks for automation)
- **OpenAI Whisper API**: Voice transcription for in-app capture ($0.006/min)
- **Google Calendar**: Morning briefing pulls today's schedule to surface relevant notes

### Content Generation (architect for later, don't build yet)
- Select insights → generate platform-specific posts (LinkedIn, blog, Twitter)
- Weekly themes → draft posts
- Content links back to source insights in knowledge graph (not in published content)
- Voice/style per platform to be developed later

### Processing Pipeline
```
Raw input (file drop, API, voice)
    → Transcription (if audio)
    → Claude processing (enhanced note + insight extraction)
    → User review/edit (synchronous, can revisit later)
    → Index update (SQLite: tags, embeddings, FTS)
    → Graph update (new connections via shared tags)
```

### Daily Workflow the Software Supports
- **Morning**: Surface notes relevant to today's calendar + recent themes
- **Capture**: Drop files to /raw/ or record in-app; auto-process
- **Review**: Read enhanced notes + insights, edit, approve
- **Evening**: Reflection based on day's activities and connections
- **Weekly**: Synthesis across the week's notes
- **Periodic**: Tag health checks, connection discovery

### 8 Life Pillars (from daily-practice.md)
1. The Studio Thrives — Parable Labs, ventures, client work
2. Wealth Compounds for Lineage — Financial sovereignty
3. Move Vitally to 112 — Dance, strength, longevity
4. Live Ecological Daily — Permaculture, garden
5. Practice Peace, Work Love — Dharma, meditation
6. Capture, Compound, Transmit — This knowledge graph
7. Zuri Thrives, Community Holds, Partnership-Ready
8. Inner Life Tended — Emotional fluency, somatic work

## Working Agreements
- Monifa is learning to build software with Claude Code — explain patterns as we build
- Build incrementally — working software at each step
- Don't over-engineer — add complexity only when needed
- Architect for extensibility (content generation, new integrations) without building it prematurely
