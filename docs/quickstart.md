# Quick start

Install [uv](https://docs.astral.sh/uv/). The default interpreter comes
from [`.python-version`](../.python-version), and the package declares its
supported Python range in [`pyproject.toml`](../pyproject.toml).

Run from the root of a disposable checkout. Regeneration writes into
`output/`, which contains committed release snapshots in this repository.

```bash
uv sync --locked --extra dev
uv run --no-sync python scripts/90_build_storybook_pdf.py
uv run --no-sync pytest tests/ --cov=src --cov-fail-under=90
```

Open `output/pdf/the-shape-between.pdf` and
`output/figures/storybook_contact_sheet.png`. The final builder renders
every story page before assembling the PDF. The separate page scripts
are useful for iterating on one image. See the
[rendering pipeline](rendering_pipeline.md) and the
[descriptive manuscript build](manuscript/README.md).
