# Output Conventions

Generated artifacts live under `output/`. The tracked publication snapshot
is tied to a release. Use a disposable checkout for regeneration.

| Path | Produced by | Contents |
|------|-------------|----------|
| `output/pdf/the-triplicate.pdf` | `20_render_newspaper.py` | the 12-page newspaper (primary artifact) |
| `output/figures/*.png` | `10_generate_figures.py` | halftone scenes + charts referenced by pages |
| `output/data/render_report.json` | `20_render_newspaper.py` | `page count, fit result, oversets, and per-page layout audit` |
| `output/reports/render_summary.txt` | `20_render_newspaper.py` | human-readable render summary |
| `output/pdf/<manuscript>.pdf` | Stage-03 infra renderer | descriptive manuscript PDF |
| `output/web/`, `output/slides/` | Stage 03 | manuscript HTML and slide decks |
| `output/reports/validation_report.md`, `output/reports/rendered_provenance.json` | Stage 04 | validation and render provenance |

`output_basename` in `content/edition.yaml` controls the newspaper filename.

The render report is the machine-checkable contract: a CI gate should assert
`page_count == 12` and `all_pages_fit == true`.
