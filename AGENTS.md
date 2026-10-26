# AGENTS - `template_storybook`

`template_storybook` is a public, standalone exemplar for full-page creative
PDFs. It renders a deterministic illustrated storybook from `content/story.yaml`
using project-local source code and thin Stage-02 scripts.

## Contract

- `content/story.yaml` owns the story text, cast, page order, overlay choice,
  and palettes.
- `src/storybook/` owns character generation, page validation, illustration,
  text overlay, accessible page metadata, contact-sheet generation, manifest
  writing, and PDF assembly.
- `scripts/` may only select a page or final artifact and call `src/storybook/`.
- `output/` is generated; regenerate it through scripts rather than editing it.

Configuration entry points are `content/story.yaml` and
`manuscript/config.yaml`; keep `manuscript/config.yaml.example` aligned for
forks.

For project-scale decisions and negative controls, follow the repository memory
and decision-record rule in the monorepo's `docs/rules/memory_and_decision_records.md`
(not present in this standalone mirror).

## Verification

Run from this repository root:

```bash
uv run --extra dev pytest tests/ --cov=src --cov-fail-under=90
for script in scripts/10_render_cover.py scripts/2?_render_page_*.py scripts/3?_render_page_*.py scripts/90_build_storybook_pdf.py; do
  uv run python "$script"
done
```

The monorepo pipeline commands (`scripts/pipeline/stage_0*.py
--project templates/template_storybook`) run this same script list plus the
descriptive-manuscript stages; they are unavailable in this standalone mirror.

After illustration changes, inspect the full PDF and the generated
`output/figures/storybook_contact_sheet.png`.

## Publishing

This project is public and forkable, but the bundled story is illustrative
fiction. Do not present it as a sourced children's book, field study, or real
child-development intervention.
