# Quickstart

## Prerequisites

```bash
uv sync --extra dev
```

provides `reportlab`, `Pillow`, `matplotlib` and `pyyaml` (plus `pytest` for the
suite); in the monorepo the workspace `.venv` already provides them. No system
tools are required to produce the newspaper PDF.

## Render the paper

```bash
uv run python scripts/10_generate_figures.py     # halftone scenes + charts → output/figures/
uv run python scripts/20_render_newspaper.py      # 12-page PDF → output/pdf/the-triplicate.pdf
open output/pdf/the-triplicate.pdf
```

Run these from this repository's root; in the monorepo run them after
`cd projects/templates/template_newspaper`.

Or in one call, programmatically:

```python
from pathlib import Path
from newspaper.engine import build_and_render
r = build_and_render(Path("."))           # run from the project dir
print(r.page_count, r.all_pages_fit)      # 12 True
```

## Inspect it like the engine's authors do

```bash
pdfinfo output/pdf/the-triplicate.pdf | grep -E 'Pages|Page size'
pdftoppm -png -r 100 output/pdf/the-triplicate.pdf /tmp/triplicate
open /tmp/triplicate-01.png
```

## Run via the repository orchestrator

```bash
# from the monorepo checkout root
./run.sh --project templates/template_newspaper --pipeline
```

Stage 02 runs the `scripts/` (figures + newspaper render); Stage 03 renders the
descriptive `manuscript/`.

## Tests & checks

```bash
python -m pytest                  # full suite
python -m pytest --cov=newspaper  # ~99% branch coverage (gate: 90%)
mypy src/newspaper                # clean
ruff check src scripts tests      # clean
```
