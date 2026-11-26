---
name: template-newspaper
description: Newspaper layout engine exemplar — 12-page tabloid/broadsheet PDF from structured YAML via pure-Python ReportLab.
version: 1.0.2
author: docxology
license: MIT
tags: [exemplar, layout, reportlab, typography]
---

# template-newspaper

Project-scoped skill for the standalone newspaper exemplar. Load this when
working in the project.

## When to Use

- Working inside the `template_newspaper` exemplar — running scripts, editing source,
  or regenerating outputs.
- Forking this exemplar as the starting scaffold for a new research project.
- Validating that the exemplar's contracts (thin-orchestrator, layer boundaries,
  no-mocks testing) still hold after changes.

## Quick Reference

```bash
# From the standalone repository root
uv sync --locked --extra dev
uv run pytest --cov=src --cov-fail-under=90
uv run python scripts/00_preflight.py
uv run python scripts/10_generate_figures.py
uv run python scripts/20_render_newspaper.py
```

## Pitfalls

- **Keep scripts thin.** Business logic belongs in `src/` or shared
  `infrastructure/`, not in `scripts/`.
- **No mocks.** All tests must use real data, real files, and real
  computation.
- **Outputs are disposable.** Never hand-edit `output/` — regenerate from
  source and config.
- **Render the manuscript from Template.** Use the [pinned runner and registration instructions](../../../docs/quickstart.md#render-the-descriptive-manuscript).

## Cross-refs

- Project contract: [`AGENTS.md`](../../../AGENTS.md)
- README: [`README.md`](../../../README.md)
- TODO: [`TODO.md`](../../../docs/TODO.md)
- Exemplar roster: [`projects/AGENTS.md`](https://github.com/docxology/template/blob/87704feae0a4490ef7dad9c931d268e8c30273c2/projects/AGENTS.md)
