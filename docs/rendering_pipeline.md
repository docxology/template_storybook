# Rendering Pipeline

Standalone, run the page scripts in order (the list in `manuscript/config.yaml`):

```bash
for script in scripts/10_render_cover.py scripts/2?_render_page_*.py scripts/3?_render_page_*.py scripts/90_build_storybook_pdf.py; do
  uv run python "$script"
done
```

In the monorepo, Stage 02 runs the same list from `manuscript/config.yaml`
through `--project templates/template_storybook`.

The cover and each numbered page render first as PNG files in
`output/figures/storybook_pages/`. The final script assembles those images into
`output/pdf/the-shape-between.pdf`, writes the deterministic inspection sheet
to `output/figures/storybook_contact_sheet.png`, and writes manifest files under
`output/data/` and `output/reports/`.
