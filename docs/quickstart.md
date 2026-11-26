# Build and verify The Triplicate

## Build the specimen edition

Install Python 3.11 or later and uv. From the repository root, install from the
lockfile and run the three scripts in order. Use a disposable checkout for
regeneration because these commands overwrite files under `output/`.

```sh
uv sync --locked --extra dev
uv run python scripts/00_preflight.py
uv run python scripts/10_generate_figures.py
uv run python scripts/20_render_newspaper.py
```

[Edition YAML](../content/edition.yaml) selects the page list, trim, column
geometry, and output basename. The shipped edition writes
`output/pdf/the-triplicate.pdf`, thirteen generated images under
`output/figures/`, and a fit report at `output/data/render_report.json`.
The render command exits nonzero when any page has overset text.

With Poppler installed, check dimensions and rasterize every page:

```sh
pdfinfo output/pdf/the-triplicate.pdf
preview=$(mktemp -d)
pdftoppm -png -r 100 output/pdf/the-triplicate.pdf "$preview/page"
```

Inspect all twelve raster pages for missing art, sparse or clipped columns, and
unintended font substitutions. Check `all_pages_fit` in the render report as
well. The [edition roster](edition.md) identifies what to expect on each page.

## Run the native checks

```sh
uv run pytest --cov=src --cov-fail-under=90
uv run --with mypy --with types-PyYAML --with types-reportlab mypy src/newspaper
uv run --with ruff ruff check src scripts tests
lychee --offline README.md STANDALONE.md AGENTS.md 'docs/**/*.md'
```

The locked pytest run is the runtime gate. The opt-in static checks currently
report existing type and lint findings in the engine.

## Render the descriptive manuscript

Install Python 3.15, Pandoc 3.14, pandoc-crossref v0.3.26b, and XeLaTeX with
the [template TeX packages](https://github.com/docxology/template/blob/87704feae0a4490ef7dad9c931d268e8c30273c2/README.md#L565-L574).
Set `NEWSPAPER_CHECKOUT` to the absolute path of the checkout to render.
Run the commands from a parent directory where you can create a runner and
test the standalone sources.

```sh
git clone https://github.com/docxology/template.git newspaper-template-runner
cd newspaper-template-runner
git checkout --detach 87704feae0a4490ef7dad9c931d268e8c30273c2
uv sync --locked --python 3.15 --group rendering
project=projects/working/template_newspaper
mkdir -p projects/working
git -C "$NEWSPAPER_CHECKOUT" worktree add --detach "$PWD/$project" HEAD
mv "$project/output" "$(mktemp -d)/published-output"
uv run python "$project/scripts/00_preflight.py"
uv run python "$project/scripts/10_generate_figures.py"
uv run python "$project/scripts/20_render_newspaper.py"
uv run python scripts/pipeline/stage_03_render.py --project working/template_newspaper
uv run python scripts/pipeline/stage_04_validate.py --project working/template_newspaper
```

The combined manuscript PDF shares `$project/output/pdf/` with the separately
generated specimen PDF. The manuscript also writes combined HTML at
`$project/output/web/index.html`, slide decks under `$project/output/slides/`,
and validation and provenance reports under `$project/output/reports/`.
