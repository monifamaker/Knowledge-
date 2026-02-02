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

## Open Questions

1. **Insight note file naming**: `YYYY-MM-DD-insight-slug.md`? Or use a unique ID? Slugs are human-readable but may collide. IDs are stable but opaque.
2. **Frontmatter schema**: What fields does an insight note need? (parent_note, tags, date_extracted, source, pillar?)
3. **Embedding model**: Which sentence embedding model for semantic search? `all-MiniLM-L6-v2` is lightweight. Larger models give better results.
4. **Frontend framework**: React? Svelte? Next.js? Depends on mobile strategy (React → React Native path is natural).
5. **Fathom/Wispr API availability**: Need to verify what's possible with current plans.
