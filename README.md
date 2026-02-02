# Knowledge-

A voice-driven personal knowledge management system built on Zettelkasten principles. Voice notes are transcribed, processed through Claude for coherence and tagging, and connected into an emergent knowledge graph through a consistent tag library.

## How It Works

```
Voice Note → Transcription → Claude Processing → Tagged Note → Knowledge Graph
                                    ↕
                              Tag Library
```

1. **Capture** — Leave voice notes while thinking, reading, listening, or reflecting
2. **Process** — Transcripts go through Claude to make them coherent and complete while preserving voice
3. **Tag** — Apply hashtags from the existing tag library, adding new tags only when genuinely needed
4. **Connect** — Tags create links between notes, building a knowledge graph automatically
5. **Reflect** — Daily practice of reading and reflecting on notes, generating new notes from insights

## Core Insight

**Filing is the bottleneck.** Most knowledge systems fail at organization, not capture. This system replaces folders with tags — each note gets 5-15 tags creating multiple retrieval pathways. A note about a client meeting connects to a note about personal boundaries connects to a podcast reflection, because they share tags.

Tags aren't categories. They're hyperlinks. The tag library enforces consistency (preventing `#Boundaries` one day and `#BoundaryWork` the next), and new tags emerge organically only when content covers genuinely new territory.

## Source Integration

Voice notes often respond to specific content — podcasts, books, articles, conversations. The system tracks what sparked the thinking:

- Cite the source so the graph includes what you're learning from
- Enable context retrieval so an agent can pull in the original transcript, PDF, or article
- Link sources to tags so you can see which inputs inform different areas

## What Emerges Over Time

- **Tag clusters** — where your attention concentrates
- **Connection patterns** — relationships between ideas you didn't consciously link
- **Evolution threads** — how thinking develops across weeks and months
- **Critical mass moments** — when enough notes share a tag that synthesis becomes possible

## Processing Principles

1. Preserve authentic voice — coherent and complete, not sanitized
2. Apply existing tags first — always check the tag library before creating new ones
3. Create new tags sparingly — only for genuinely new territory
4. Include source references — when responding to specific content
5. Suggest connections — flag related notes or themes when patterns emerge

## System Components

### Tag Library
- Master list of all tags with descriptions
- Enforces naming consistency across all notes
- Grows organically as new topics emerge
- Serves as a map of everything you think about

### Note Store
- Processed notes with metadata (date, source, tags)
- Full-text searchable
- Retrievable by tag, date, source, or semantic similarity

### Knowledge Graph
- Notes are nodes; shared tags are edges
- Visualize clusters, connections, and isolated notes
- Query: "show me everything connected to #ConsultingMethodology"
- Surface notes that share tags but haven't been explicitly linked

### Processing Pipeline
- Transcription (Whisper or upload transcript directly)
- Claude processing with tag library context
- Tag validation and assignment
- Storage and graph update

## Implementation Phases

### Phase 1 — Foundation
- Tag library format and initial seed tags
- Note schema (content, metadata, tags, sources)
- Storage layer (SQLite for notes + metadata, FTS5 for search)
- Claude processing prompt with tag library injection

### Phase 2 — Core Workflow
- Voice note ingestion (file upload, transcription via Whisper)
- Processing pipeline: transcript → Claude → tagged note
- Tag library management (list, add, merge, rename)
- Basic search (full-text, by tag, by date)

### Phase 3 — Knowledge Graph
- Graph construction from tag relationships
- Visualization (Cytoscape.js or D3.js)
- Cluster detection and connection surfacing
- "Related notes" suggestions

### Phase 4 — Reflection Tools
- Daily review interface: recent notes, new connections
- Tag cluster analysis (attention mapping)
- Evolution threads (trace thinking over time on a tag)
- Synthesis prompts when tag clusters reach critical mass

### Phase 5 — Source & Context
- Source library (podcasts, books, articles, conversations)
- Source-to-note linking
- Agent-assisted context retrieval (pull in original content)
- Export (Markdown, JSON, Obsidian-compatible)

## Project Structure (Planned)

```
Knowledge-/
├── api/                # FastAPI endpoints
├── core/
│   ├── transcribe.py   # Whisper integration
│   ├── process.py      # Claude processing pipeline
│   ├── tags.py         # Tag library management
│   ├── graph.py        # Knowledge graph operations
│   └── search.py       # Full-text + tag + semantic search
├── models/             # Data models / schemas
├── storage/            # SQLite + graph persistence
├── tags/               # Tag library definition
├── web/                # Frontend (graph visualization, review UI)
├── tests/
└── requirements.txt
```

## The Daily Practice

- **Capture throughout the day** — voice notes whenever insights arise
- **Process in batches** — run transcripts through Claude to refine and tag
- **Reflect daily** — review recent notes, notice connections, generate new notes
- **Periodic review** — examine tag clusters, identify synthesis opportunities

## What Good Output Looks Like

A well-processed note:
- Reads clearly while maintaining original voice and thinking style
- Includes 5-15 tags creating multiple retrieval pathways
- References sources when responding to specific content
- Connects to the existing knowledge graph through shared tags
- Preserves incomplete or evolving thoughts rather than forcing resolution

The measure of success isn't polish — it's whether the note connects to other notes and is findable when relevant.
