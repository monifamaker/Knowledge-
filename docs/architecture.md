# Architecture Decisions

Captured during planning session, 2026-02-02.

---

## ADR-001: Hybrid Storage (Markdown + SQLite Index)

**Context**: Notes need to be portable, human-readable, and Obsidian-compatible, but also searchable by tag, date, semantic similarity, and full-text.

**Decision**: Markdown files are the source of truth. SQLite is a derived index that can be rebuilt from files at any time.

**Consequences**:
- Files are never locked inside a database
- Need a sync mechanism to keep index current with file changes
- YAML frontmatter on insight notes carries structured metadata
- FTS5 for full-text search, embeddings table for semantic search

---

## ADR-002: Two-Tier Note Architecture (Enhanced + Insights)

**Context**: The prototype produces "enhanced notes" — structured versions of raw inputs. But these remain source-bound. Ideas can't freely recombine across sources.

**Decision**: Processing produces two outputs:
1. **Enhanced note** — one per raw input, in `/notes/enhanced/`
2. **Insight notes** — atomic ideas extracted from the enhanced note, in `/notes/insights/`

**Insight note properties**:
- One idea per note
- Full sentences, standalone (understandable without parent)
- Framed as claim/observation/question, not summary
- Links back to parent enhanced note
- Independently tagged (5-15 tags)

**Consequences**:
- Insight extraction happens during processing (synchronous with user review)
- User can revisit and edit insights later
- Insights are the primary unit of recombination (for synthesis, content generation, pattern discovery)
- Graph connectivity increases dramatically (more nodes with independent tags)

---

## ADR-003: Build Order (API → Web → Watcher → Mobile)

**Context**: Multiple possible starting points. User wants morning/evening rituals, auto-processing, and mobile capture.

**Decision**: Build in this order:
1. API backend (processing pipeline, storage, search)
2. Web UI (review, morning/evening, graph visualization)
3. File watcher + integrations (auto-processing, Fathom, Wispr, Calendar)
4. Mobile app (capture + read + review)

**Rationale**:
- API backend is required by everything else
- Web UI enables the highest-value use cases (review, morning/evening rituals)
- File watcher is a feature added to the backend, not a separate architecture
- Mobile is a frontend to the same API — build last when the API is stable

---

## ADR-004: Synchronous Insight Review with Deferred Editing

**Context**: Should insight extraction be fully automatic or require approval?

**Decision**: Synchronous — user reviews proposed insights during processing. But insights can be edited, deleted, or added later.

**Rationale**:
- The review step is where the user learns from their own thinking
- Fully automatic extraction risks low-quality insights accumulating
- Deferred editing means the system doesn't block on perfection

---

## ADR-005: Content Generation Deferred, Architecture Prepared

**Context**: User wants to generate LinkedIn, blog, and Twitter posts from recombinant insights. Not ready to build yet (no platform voice/style defined).

**Decision**: Don't build content generation now. Architect so it can be added without refactoring:
- Insights are independently tagged and linkable — they're the input to content generation
- Content pieces will link back to source insights in the knowledge graph (not in published content)
- The content generation module will be a separate concern that reads from the insight store

**What this means for current architecture**:
- Insight notes need stable IDs (for linking from future content records)
- The graph model needs to support a "content" node type eventually
- No premature abstractions — just ensure insights are well-structured

---

## ADR-006: Claude API for All AI Processing

**Context**: Multiple AI tasks — note enhancement, insight extraction, tagging, synthesis, morning briefing, content generation (future).

**Decision**: Use Claude API (not local models) for all AI processing.

**Rationale**:
- Quality matters more than cost for a personal system
- Claude already has the context of the system prompt and tag library (proven in prototype)
- Simplifies the stack (no GPU requirements, no model management)
- Processing volume is low (a few notes per day)

---

## ADR-007: Google Calendar Integration for Context-Aware Briefings

**Context**: Morning briefing is more valuable when it knows the day's schedule.

**Decision**: Integrate Google Calendar API to pull today's events, then surface notes related to meeting participants, topics, and projects.

**Deferred**: Build in Phase 3 (file watcher & automation). The morning briefing can work without it initially (surfacing recent notes and themes).

---

## ADR-008: Transcription & Integration APIs

**Context**: Need transcription for voice capture in the app, and integration with meeting recording tools.

**Research findings** (2026-02-04):
- **Fathom**: Public API available on free plan. Transcripts, summaries, action items, webhooks. 60 req/min rate limit.
- **Wispr Flow**: API exists but requires exclusive access approval. Not immediately available.

**Decision**:
- Use **OpenAI Whisper API** for voice transcription ($0.006/min, excellent quality, immediate access)
- Use **Fathom API + webhooks** for meeting transcript ingestion (auto-trigger processing when meetings complete)

**Rationale**:
- Whisper API is the same underlying tech Wispr uses, no approval gates
- Fathom webhooks enable true automation — no polling required
- Design transcription as a pluggable interface; can swap providers later

**Integration architecture**:
```
Voice capture (app) → Whisper API → transcript → /raw/ → processing pipeline
Fathom webhook → fetch transcript → /raw/ → processing pipeline
```

---

## ADR-009: Insight Note Format (Naming + Frontmatter)

**Context**: Insight notes need stable IDs for linking, human-readable filenames, and structured metadata.

**Decision**: Hybrid filename with minimal frontmatter schema.

**Filename format**: `YYYY-MM-DD-XXXX-slug.md`
```
2026-02-04-7f3a-setting-boundaries-with-clients.md
└─────────────┘ └─────────────────────────────────┘
   stable ID              human-readable slug
```

- Date prefix for chronological sorting
- 4-char random suffix (`7f3a`) guarantees uniqueness
- Slug auto-generated from first ~5 words, user-editable
- ID is `YYYY-MM-DD-XXXX` portion — stable even if slug changes

**Frontmatter schema**:
```yaml
---
id: 2026-02-04-7f3a
parent: 2026-02-04-enhanced-client-meeting-acme
tags:
  - client-work
  - boundaries
  - professional-development
  - parable-labs
created: 2026-02-04
source_type: meeting
---
```

| Field | Required | Purpose |
|-------|----------|---------|
| `id` | Yes | Stable identifier for linking (matches filename prefix) |
| `parent` | Yes | Links to the enhanced note this was extracted from |
| `tags` | Yes | 5-15 tags for graph connectivity |
| `created` | Yes | Date extracted (for sorting, filtering) |
| `source_type` | Yes | `voice_note`, `meeting`, `email`, `article` — for filtering |

**Fields intentionally omitted**:
- `pillar` — tags already capture this
- `status` — adds workflow complexity; defer until needed
- `related` — let tags create connections; explicit links can come later
- `source_date` — parent note has this; don't duplicate

---

## Deferred Decisions

These don't need to be resolved before starting Phase 1:

1. **Embedding model** (Phase 1, later): Start with `all-MiniLM-L6-v2` — lightweight, good enough. Swap if quality becomes an issue.
2. **Frontend framework** (Phase 2): Decide when starting web UI. React is the safe choice if mobile React Native is likely.
