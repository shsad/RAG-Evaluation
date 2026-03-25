# Notebooks Quickstart

This folder contains notebooks for two workshops:

- **Workshop 2 — RAG Fundamentals:** chunking, retrieval, embeddings, OCR
- **Workshop 3 — RAG Evaluation:** RAGAS metrics, retrieval & generation evaluation

Start with `00_setup.ipynb` to prepare everything you need:
- API key config in `notebooks/.env`
- Python environment via `uv sync`
- Local Qdrant via Docker
- API connection test (Embeddings + Generation)

## Prerequisites

- Python 3.10+
- [`uv`](https://docs.astral.sh/uv/getting-started/installation/)
- Docker (running)

## 1) Prepare the notebooks environment

Preferred (repository-level `.venv-notebooks` + reusable Jupyter kernel):

From the repository root:

```bash
./scripts/setup_notebooks_kernel.sh
```

Optional extras for OCR/VLM:

- Normal users (VLM support):
```bash
./scripts/setup_notebooks_kernel.sh --extra vlm
```

- Mac users (Apple Silicon, OCRMac + VLM):
```bash
./scripts/setup_notebooks_kernel.sh --extra mac_vlm
```

This will:
1. Create/use `./.venv-notebooks` in the repository root.
2. Install dependencies from `notebooks/pyproject.toml` into that root environment.
3. Register a Jupyter kernel named `workshop-ragv2` (display name `Python (workshop-ragV2)`).

Legacy/local notebooks-only environment (still supported):

Open folder `notebooks/` in VS Code, then run:

```bash
uv venv
source .venv/bin/activate
uv sync
```

## 2) Open and run `00_setup.ipynb`

Open `notebooks/00_setup.ipynb` in VS Code.

In the top-right of the notebook:
1. Click `Select Kernel`.
2. Click `Jupyter Kernel...`.
3. Pick `Python (workshop-ragV2)` (`.venv-notebooks/bin/python`).

Then run the cells from top to bottom.

What the notebook does:
1. Creates `notebooks/.env` if missing.
2. Validates that `OPENAI_API_KEY` is set.
3. Starts or reuses a local Docker container named `qdrant`.
4. Tests HPI API connection (Embeddings + Generation).
5. Checks Workshop 3 data files (CSV + PDF).

## 3) Configure your API key

After the notebook creates `.env`, edit `notebooks/.env`:

```env
OPENAI_API_KEY=your_real_key_here
OPENAI_API_BASE=https://api.aisc.hpi.de/
```

Then rerun the API key validation cell in `00_setup.ipynb`.

## Workshop Notebooks

### Workshop 2 — RAG Fundamentals

| # | Notebook | Topic |
|---|----------|-------|
| 01 | `w2_01_chunking_and_retrieval.ipynb` | Chunking strategies and retrieval |
| 02 | `w2_02_embedding_models.ipynb` | Embedding model comparison |
| 03 | `w2_03_real_world_datentypen.ipynb` | Real-world data types |
| 04 | `w2_04_ocr_docling_vlm_comparison.ipynb` | OCR: Docling vs VLM comparison |

### Workshop 3 — RAG Evaluation with RAGAS

| # | Notebook | Topic | Duration |
|---|----------|-------|----------|
| 01 | `w3_01_intro_end_to_end.ipynb` | End-to-end RAG + RAGAS intro on a simple website | 30 min |
| 02 | `w3_02_ingestion.ipynb` | PDF → Docling → Chunks → Embeddings → Qdrant | 20 min |
| 03 | `w3_03_retrieval_evaluation.ipynb` | Context Precision & Context Recall, TOP_K experiment | 40 min |
| 04 | `w3_04_generation_evaluation.ipynb` | Answer Correctness & Faithfulness, prompt experiment | 40 min |

Workshop 3 notebooks (02–04) share settings via `config.py`. See the config file for available parameters (dataset, chunking mode, embedding model, etc.).

## Qdrant

Dashboard: http://localhost:6333/dashboard

Useful Docker commands:

```bash
docker ps --filter name=qdrant
docker logs qdrant --tail 100
docker start qdrant
```

## Troubleshooting

Kernel/interpreter does not appear:

```bash
./scripts/setup_notebooks_kernel.sh --skip-sync
```

Then reload VS Code and select kernel `Python (workshop-ragV2)`.

`uv: command not found`:
- Install uv, then restart your terminal.

Qdrant start fails:
- Confirm Docker is running.
- Confirm port `6333` is free.
- Check existing container state with `docker ps -a --filter name=qdrant`.

**RAGAS `InstructorRetryException` / `max_tokens` errors:**
- The evaluator LLM may need a higher `max_tokens` — especially reasoning models like `gpt-oss-120b`.
- Increase `max_tokens` in the `llm_factory()` call (already set to 8192–65536 in notebooks 03/04).

**Embedding `batch_size` errors (413):**
- `minilm-embedding` has a max batch size of 32 (configured in `config.py`).
- If you add a new embedding model, add its constraints to the `_EMBED_MODELS` registry in `config.py`.
