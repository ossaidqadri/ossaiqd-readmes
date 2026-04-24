# Lexa RAG — Pakistani Legal Retrieval-Augmented Generation System

A production-grade RAG pipeline for querying Pakistani law, built with Qdrant vector database, Isaacus Kanon 2 embeddings, and a Next.js 16 frontend.

## Overview

Lexa RAG provides accurate, citeable answers from Pakistani legal documents. It uses vector similarity search over chunked law texts, with metadata structured for precise citation handles. Designed for legal professionals who need to navigate the Constitution, Anti-Rape Investigation and Trial Act 2021, and Anti-Terrorism Act 1997.

`★ Insight ─────────────────────────────────────`
**Why RAG over fine-tuning?** Legal documents change frequently —宪法 amendments, new acts. RAG lets you update the knowledge base without retraining. Fine-tuning is expensive and can't guarantee recall of specific clauses.
`─────────────────────────────────────────────────`

## System Architecture

```
PDF Sources → LlamaParse → Markdown → Chunk Scripts → JSON Chunks
                                                      ↓
                                          Isaacus Kanon 2 (embed)
                                                      ↓
                                              Qdrant Vector DB
                                                      ↓
                                          Next.js Chat UI (RAG query)
```

### Components

| Layer | Technology | Role |
|---|---|---|
| **Frontend** | Next.js 16 + prompt-kit | Chat interface with markdown rendering |
| **Embeddings** | Isaacus Kanon 2 (1792-dim) | `task="retrieval/document"` for storage-optimized vectors |
| **Vector DB** | Qdrant | Cosine similarity search with payload indexes |
| **Chunking** | Python + regex | Structure-aware splitting by Article/Section |
| **Parsing** | LlamaParse (premium) | PDF → Markdown with metadata extraction |

## Document Corpus

| Document | Source | Chunking Strategy |
|---|---|---|
| **Constitution of Pakistan (1973)** | `the-constitution-of-the-islamic-republic-of-pakistan.pdf` | Split by `## Article X.` header |
| **Anti-Rape Investigation and Trial Act 2021** | `the-anti-rape-investigation-and-trial-act-2021.pdf` | Split by `## Section X.` header, sub-subsection `(1)` splitting for long sections |
| **Anti-Terrorism Act 1997** | `the-anti-terrorism-act-1997.pdf` | Split by `## Section X.` header, preserves First Schedule (proscribed orgs table) |

## Data Model

Each chunk carries metadata for legal citation:

```json
{
  "content": "Article 6: High treason...",
  "metadata": {
    "document_id": "uuid",
    "act_name": "Constitution",
    "act_year": "1973",
    "section_number": "6",
    "jurisdiction": "Pakistan",
    "source_type": "Constitution",
    "citation_handle": "[Constitution, Art.6, Pg.1]"
  }
}
```

**Citation handles** (`citation_handle`) are deterministic — derived from `citation_handle` or content hash via UUID v5, preventing duplicate vectors on re-ingestion.

`★ Insight ─────────────────────────────────────`
**UUID v5 with DNS namespace** — The same content always generates the same ID. Re-running ingestion is idempotent — no duplicate vectors in Qdrant.
`─────────────────────────────────────────────────`

## Chunking Pipeline

Scripts in `rag/` handle each document type:

- `process_constitution.py` — LlamaParse → `constitution_production_chunks.json`
- `process_ata.py` → `chunk_anti-terror.py` — Anti-Terrorism Act: parse → structure-aware chunk → JSON
- `chunk_antirape.py` — Anti-Rape Act: regex section split with subsection splitting for sections >2000 chars

Long sections get split by subsection marker `(1)`, `(2)` etc., ensuring chunks stay under ~2000 chars for embedding quality.

## Ingestion (`rag/ingest.py`)

```python
isaacus_client.embeddings.create(
    model="kanon-2-embedder",
    texts=batch_texts,
    task="retrieval/document"  # optimizes for storage/retrieval, not relevance
)
```

- **Batch size**: 20 chunks per API call
- **Collection**: `the-constitution-of-the-islamic-republic-of-pakistan`
- **Qdrant payload indexes** on: `act_name`, `section_number`, `citation_handle`

## Frontend (`next/`)

Next.js 16 app with:
- `@ai-sdk/react` for streaming chat
- `prompt-kit` UI components (Markdown renderer, tooltips, avatars)
- Dark theme (`#212121` background, `#171717` sidebar)
- Suggested prompts for Constitution queries
- Copy-to-clipboard on assistant responses

## Environment Variables

```env
ISAACUS_API_KEY=       # Isaacus Kanon 2 API key
LLAMA_CLOUD_API_KEY=   # LlamaParse for PDF parsing
QDRANT_URL=            # Qdrant server URL (default: http://localhost:6333)
QDRANT_API_KEY=        # Qdrant API key (optional for local)
```

## Quick Start

```bash
# 1. Parse and chunk documents
cd rag
python process_constitution.py

# 2. Ingest to Qdrant
python ingest.py

# 3. Start frontend
cd next
bun dev
```

## Key Files

```
rag/
├── process_constitution.py   # LlamaParse + structure-aware chunking
├── chunk_constitution.py     # Alternative regex-based constitution chunker
├── chunk_antirape.py         # Anti-Rape Act section splitter
├── chunk_anti-terror.py      # Anti-Terrorism Act parser + chunker
├── ingest.py                 # Qdrant upsert with Kanon 2 embeddings
└── knowledge-source-docs/   # Source PDFs and parsed markdown

next/
├── app/page.tsx             # Chat UI (main entry)
├── app/layout.tsx           # Root layout
└── components/              # prompt-kit + shadcn/ui components
```