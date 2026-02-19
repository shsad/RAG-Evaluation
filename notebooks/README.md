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

From the repository root:

```bash
cd notebooks
uv venv
source .venv/bin/activate
uv sync
```

## 2) Open and run `00_setup.ipynb`

- Open `notebooks/00_setup.ipynb` in VS Code or Jupyter Lab.
- Select the interpreter/kernel from `notebooks/.venv` (or let VS Code auto-detect it after `uv sync`).
- Run cells top to bottom.

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
cd notebooks
uv sync
```

Then reload VS Code and select the Python interpreter from `notebooks/.venv`.

`uv: command not found`:
- Install uv, then restart your terminal.

Qdrant start fails:
- Confirm Docker is running.
- Confirm port `6333` is free.
- Check existing container state with `docker ps -a --filter name=qdrant`.

## Next notebook

After setup completes, continue with:
- `notebooks/01_chunking_and_retrieval.ipynb`
