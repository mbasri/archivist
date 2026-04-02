# Archivist

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**Turn your NAS into a knowledge base you can chat with.**

Archivist is a self-hosted RAG pipeline that indexes any file from your NAS — PDF, Office documents, images, audio, video — into a vector database, and lets you chat with your data through a local or cloud LLM.

---

## How it works

```
NAS (any format)
      │
      ▼
 Conversion → text          Docling (docs, images) · Whisper (audio, video)
      │
      ▼
 Chunking + Embedding       local (Ollama) or cloud (OpenAI, AWS Bedrock)
      │
      ▼
 Vector database            Qdrant
      │
      ▼
 Chatbot                    Open WebUI
```

---

## Stack

| Component       | Technology                          |
|-----------------|-------------------------------------|
| File conversion | [Docling](https://github.com/DS4SD/docling) |
| Transcription   | [Whisper](https://github.com/openai/whisper) |
| Vector database | [Qdrant](https://qdrant.tech)       |
| Embeddings      | [Ollama](https://ollama.com) · OpenAI · AWS Bedrock |
| LLM             | [Ollama](https://ollama.com) · OpenAI · AWS Bedrock |
| Chatbot UI      | [Open WebUI](https://openwebui.com) |
| Indexing        | Jupyter notebooks (Python)          |

---

## Project structure

```
archivist/
├── docker-compose.yml          # Qdrant (vector database) + Ollama (embeddings + LLM)
├── notebooks/
│   └── indexing.ipynb          # Full pipeline: scan → chunk → embed → index into Qdrant
├── index.json                  # Tracks indexed files by hash (auto-generated)
└── README.md
```

---

## Getting started

### Prerequisites

- Docker and Docker Compose
- Python 3.12+ and Jupyter

### 1. Start the stack

```bash
docker compose up -d

# Pull the embedding model (once)
docker exec archivist-ollama ollama pull nomic-embed-text
```

Qdrant dashboard available at `http://localhost:6333/dashboard`.

### 3. Run the indexing pipeline

```bash
jupyter notebook notebooks/
```

Open `indexing.ipynb` and run all cells.

> The notebook creates its own venv and installs dependencies automatically on first run.

---

## Configuration

All settings are set in the **Setup Variables** cell at the top of each notebook — no config file needed.

| Variable | Description | Default |
|----------|-------------|---------|
| `nas_path` | Path to the directory to index | `` |
| `embeddings_model` | Ollama embedding model | `nomic-embed-text` |
| `qdrant_host` | Qdrant host | `localhost` |
| `qdrant_port` | Qdrant port | `6333` |
| `ollama_host` | Ollama host | `localhost` |
| `ollama_port` | Ollama port | `11434` |

---

## Supported file formats

| Category  | Extensions |
|-----------|-----------|
| Documents | `.pdf` `.txt` `.md` `.docx` `.pptx` `.xlsx` `.odt` `.odp` `.ods` `.csv` |
| Images    | `.jpg` `.jpeg` `.png` `.gif` `.bmp` `.tiff` `.webp` `.svg` |
| Audio     | `.mp3` `.wav` `.flac` `.m4a` `.ogg` `.aac` |
| Video     | `.mp4` `.mkv` `.avi` `.mov` `.webm` `.flv` |

---

## Roadmap

- [x] Scan NAS and list files by category
- [x] Incremental indexing — skip already indexed files (hash-based)
- [x] Chunk, embed and index plain text files (`.txt`, `.md`, `.csv`)
- [x] Docker Compose (Qdrant + Ollama)
- [ ] Convert documents to text (Docling) — PDF, PPTX, DOCX, images
- [ ] Transcribe audio and video (Whisper)
- [ ] Open WebUI integration

---

## Author

[Mohamed BASRI](https://github.com/mbasri)

## License

This is free and unencumbered software released into the public domain - see the [LICENSE](./LICENSE) file for details.
