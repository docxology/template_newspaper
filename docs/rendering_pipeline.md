# Rendering pipeline

## Specimen edition

The project scripts run in lexicographic order in Template Stage 02 or directly
from the standalone checkout. The [quickstart](quickstart.md#build-the-specimen-edition)
shows how to run them.

| Script | Job | Output |
|--------|-----|--------|
| [Preflight](../scripts/00_preflight.py) | Import dependencies and validate the edition | Exit status |
| [Figure generation](../scripts/10_generate_figures.py) | Build procedural scenes, charts, and display-ad art | `output/figures/*.png` |
| [Edition renderer](../scripts/20_render_newspaper.py) | Build the edition and report overset | `output/pdf/the-triplicate.pdf`, `output/data/render_report.json`, `output/reports/render_summary.txt` |

The edition renderer loads the YAML, registers fonts, opens an invariant
ReportLab canvas at the configured trim, renders the pages, and saves the
PDF. Its result records the page count and any dropped flowables. With the
same fonts, dependencies, and content, the invariant canvas and fixed figure
recipes support byte-for-byte comparison.

## Descriptive manuscript

The [engine manuscript](manuscript/README.md) uses the
[shared Template runner](quickstart.md#render-the-descriptive-manuscript).
Stage 03 renders the numbered source sections, bibliography, and preamble as
a combined paper, HTML, and slides. Stage 04 checks the resulting outputs
and records validation and provenance. The edition's thirteen generated
images appear in the specimen PDF. The manuscript describes their production
and contains citations to the design and software references.
