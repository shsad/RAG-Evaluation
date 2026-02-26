# Notebooks Quickstart

This folder contains workshop notebooks.

Start with `00_setup.ipynb` to prepare everything you need:
- API key config in `notebooks/.env`
- Python environment via `uv sync`
- Local Qdrant via Docker

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
3. Runs `uv sync`.
4. Starts or reuses a local Docker container named `qdrant`.
5. Verifies Qdrant is reachable on `http://localhost:6333`.

## 3) Configure your API key

After the notebook creates `.env`, edit `notebooks/.env`:

```env
OPENAI_API_KEY=your_real_key_here
OPENAI_API_BASE=https://api.aisc.hpi.de/
```

Then rerun the API key validation cell in `00_setup.ipynb`.

## Qdrant checks

Qdrant dashboard:
- `http://localhost:6333/dashboard`

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

## Next notebook

After setup completes, continue with:
- `notebooks/01_chunking_and_retrieval.ipynb`

## OCR notebook

For OCR comparison on scanned/non-machine-readable PDFs (tables + math), use:
- `notebooks/04_ocr_docling_vlm_comparison.ipynb`

It compares:
- Docling OCR (RapidOCR / optional OCRMac for macOS)
- Docling VLM pipeline
- External VLM via LiteLLM

Input PDF in this workshop:
- `notebooks/raw_data/lstm.pdf`
