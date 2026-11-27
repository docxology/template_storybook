# Storybook descriptive manuscript

This is the canonical source of the descriptive manuscript. The numbered
sections describe the story's architecture, creative methods, and
verification. The story pages come from [`content/story.yaml`](../../content/story.yaml).
The [figure registry](assets/figure_registry.json) describes the contact
sheet produced during the book build.

## Read the published manuscript

Read the [published HTML manuscript](https://6a47fd2233010f4eb138cdcd--tranquil-kleicha-0c9203.netlify.app/) or download the [v0.1.0 manuscript PDF](https://github.com/docxology/template_storybook/releases/download/v0.1.0/template_storybook_combined.pdf).

GitHub Pages deployment status is tracked in [issue #8](https://github.com/docxology/template_storybook/issues/8).

## Render and validate

Use a disposable source checkout and a separate checkout of the
[pinned template](https://github.com/docxology/template/tree/87704feae0a4490ef7dad9c931d268e8c30273c2).
Install Git, uv, Pandoc, XeLaTeX, and the PDF and Beamer dependencies in
the template [setup guide](https://github.com/docxology/template/blob/87704feae0a4490ef7dad9c931d268e8c30273c2/docs/guides/startup-and-setup.md).

From the source checkout, preserve the tracked release snapshot outside
the working tree before running the native producer and shared pipeline.

```bash
storybook_checkout="$(pwd -P)"
render_workspace="$(mktemp -d)"
mv output "$render_workspace/previous-output"

uv sync --locked --extra dev
uv run --no-sync python scripts/90_build_storybook_pdf.py
cp docs/manuscript/assets/figure_registry.json output/figures/

renderer_checkout="$render_workspace/template"
git clone https://github.com/docxology/template.git "$renderer_checkout"
git -C "$renderer_checkout" checkout --detach 87704feae0a4490ef7dad9c931d268e8c30273c2
cd "$renderer_checkout"
uv sync --locked
mkdir -p projects/working
ln -s "$storybook_checkout" projects/working/template_storybook
uv run --no-sync python scripts/pipeline/stage_03_render.py --project working/template_storybook
uv run --no-sync python scripts/pipeline/stage_04_validate.py --project working/template_storybook
```

In the storybook checkout, inspect
`output/pdf/template_storybook_combined.pdf`, `output/web/index.html`,
and the section slides under `output/slides/`. Check the descriptive
sections, citations, and contact sheet. Review
`output/reports/validation_report.md` and `output/reports/rendered_provenance.json`
for the validation verdict and source bindings.
