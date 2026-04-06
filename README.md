# Archivist

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**Turn your NAS into a knowledge base you can chat with.**

100% self-hosted RAG pipeline. Indexes files from any directory into Qdrant, then lets you query them through a local LLM via Jupyter notebooks.

---

## How it works

```
NAS (PDF, Office, text...)
      │
      ▼  01-extract.ipynb
 MarkItDown → extracted/*.md
      │
      ▼  02-indexing.ipynb
 Chunk + Embed (Ollama) → Qdrant
      │
      ▼  03-chat.ipynb
 Chat with your documents
```

---

## Stack

| | |
|---|---|
| Extraction | [MarkItDown](https://github.com/microsoft/markitdown) |
| Embeddings + LLM | [Ollama](https://ollama.com) |
| Vector database | [Qdrant](https://qdrant.tech) |
| Orchestration | [LlamaIndex](https://docs.llamaindex.ai) |

---

## Getting started

**Prerequisites:** Docker, Python 3.12+, Jupyter

```bash
# 1. Start Qdrant + Ollama
docker compose up -d

# 2. Pull models (once)
docker exec archivist-ollama ollama pull nomic-embed-text
docker exec archivist-ollama ollama pull llama3.2:1b

# 3. Configure
cp .env.example .env
# edit .env → set NAS_PATH

# 4. Run notebooks in order
jupyter notebook notebooks/
```

Qdrant dashboard: `http://localhost:6333/dashboard`

---

## Configuration

Copy `.env.example` to `.env` and set `NAS_PATH`. Everything else has sensible defaults.

| Variable | Default | Description |
|---|---|---|
| `NAS_PATH` | — | Directory to scan |
| `EMBEDDING_MODEL` | `nomic-embed-text` | Ollama embedding model |
| `LLM_MODEL` | `llama3.2:1b` | Ollama LLM |
| `CHUNK_SIZE` | `512` | Tokens per chunk |
| `COLLECTION_NAME` | `archivist` | Qdrant collection |

---

## Supported formats

`.pdf` `.docx` `.pptx` `.xlsx` `.csv` `.txt` `.md` `.html` `.epub` `.xml` `.json`

---

## Author

[Mohamed BASRI](https://github.com/mbasri)

## License

This is free and unencumbered software released into the public domain — see [LICENSE](./LICENSE).
