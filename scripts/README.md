# Storybook Scripts

Run from the project root:

```bash
uv run python scripts/10_render_cover.py
uv run python scripts/20_render_page_01.py
uv run python scripts/32_render_page_13.py
uv run python scripts/90_build_storybook_pdf.py
```

Run every script in order to build the full PDF (this mirrors the
`analysis.scripts` list in `manuscript/config.yaml`):

```bash
for script in scripts/10_render_cover.py scripts/2?_render_page_*.py scripts/3?_render_page_*.py scripts/90_build_storybook_pdf.py; do
  uv run python "$script"
done
```

The monorepo Stage-02 command runs the same list via
`--project templates/template_storybook`.
