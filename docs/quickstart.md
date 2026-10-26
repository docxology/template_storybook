# Quickstart

```bash
uv sync --extra dev
for script in scripts/10_render_cover.py scripts/2?_render_page_*.py scripts/3?_render_page_*.py scripts/90_build_storybook_pdf.py; do
  uv run python "$script"
done
open output/pdf/the-shape-between.pdf
```

Run tests:

```bash
uv run --extra dev pytest tests/ --cov=src --cov-fail-under=90
```

The monorepo pipeline commands (`scripts/pipeline/stage_0*.py --project
templates/template_storybook`) are not present in this standalone mirror.
