# Knowledge-

A personal knowledge management system that processes voice notes into a searchable knowledge graph.

## Architecture

```
Voice Notes → Transcription → Entity Extraction → Knowledge Graph → Search Interface
```

## Pipeline Stages

| Stage | Purpose | Technology |
|---|---|---|
| Capture | Record/import voice notes | File upload, mobile recorder |
| Transcription | Speech-to-text | OpenAI Whisper (local) |
| NLP Processing | Extract entities, concepts, relationships | LLM-based extraction (structured output) |
| Graph Storage | Store nodes + edges | SQLite (metadata/embeddings) + NetworkX or Neo4j (graph) |
| Search/Query | Retrieve knowledge | Semantic search (sentence embeddings) + graph traversal |

## Tech Stack

- **Backend**: Python / FastAPI
- **Transcription**: OpenAI Whisper (local, GPU-accelerated)
- **Entity Extraction**: LLM API → structured `{entities, relationships, summary}` per transcript
- **Storage**: SQLite for metadata + vector embeddings; NetworkX (lightweight) or Neo4j (production) for the graph
- **Search**: `all-MiniLM-L6-v2` sentence embeddings for semantic similarity; full-text search via SQLite FTS5
- **Frontend**: Web UI with graph visualization (Cytoscape.js or D3.js)

## Implementation Phases

### Phase 1 — Core Pipeline
- Audio file upload endpoint
- Whisper transcription
- Store transcripts + metadata in SQLite

### Phase 2 — Knowledge Extraction
- LLM-based entity and relationship extraction from transcripts
- Entity resolution (normalize duplicates via embedding similarity)
- Persist entities and relationships in graph store

### Phase 3 — Search
- Full-text search over transcripts (SQLite FTS5)
- Semantic similarity search via sentence embeddings
- Graph traversal queries ("everything connected to X")

### Phase 4 — Graph Visualization
- Interactive knowledge graph UI
- Click nodes to view source voice notes
- Filter by date, topic, entity type

### Phase 5 — Polish
- Mobile-friendly voice capture
- Incremental graph updates on new notes
- Entity deduplication and merge tools
- Export (Markdown, JSON, Obsidian-compatible)

## Key Design Decisions

1. **Local-first**: Whisper runs locally (free, private). LLM extraction can use local models or cloud APIs.
2. **Graph model**: Notes and extracted entities are nodes. Edges represent mentions, relationships, and temporal links.
3. **Entity resolution**: Normalize variants ("ML" / "machine learning") using embedding cosine similarity with a merge threshold.
4. **Incremental processing**: Each note is processed on ingest rather than in batch.

## Project Structure (Planned)

```
Knowledge-/
├── api/              # FastAPI endpoints
├── core/
│   ├── transcribe.py # Whisper integration
│   ├── extract.py    # LLM entity/relationship extraction
│   ├── graph.py      # Graph operations
│   └── search.py     # Full-text + semantic search
├── models/           # Data models / schemas
├── storage/          # SQLite + graph persistence
├── web/              # Frontend (graph visualization)
├── tests/
└── requirements.txt
```
