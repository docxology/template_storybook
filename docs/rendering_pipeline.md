# Rendering pipeline

The [book source](../content/story.yaml) declares page dimensions,
cast, ordered pages, text, captions, and scene settings. The
[manuscript configuration](manuscript/config.yaml) declares the template
analysis scripts and the descriptive render formats.

## Build the illustrated book

After [installing the lockfile](quickstart.md), call
`uv run --no-sync python scripts/90_build_storybook_pdf.py`. The builder
revalidates the YAML, removes stale PNGs from the page-image directory,
rerenders every page, and assembles the PDF. Run an individual
[page script](../scripts/README.md) to render an illustration for review.

The pinned shared template can invoke the configured script list through
Stage 02 when the project is registered there. The standalone builder
provides the artifacts consumed by the
[descriptive manuscript render](manuscript/README.md).

## Verify the artifacts

Compare the PDF and the contact sheet with the YAML page order. Use the
[manifest](../output/data/storybook_manifest.json) to inspect each page's
alt text and raster text contrast audit. Each storybook PDF page contains a
full-page image. The manifest carries its page descriptions separately.

The [output map](output_conventions.md) locates the book and its supporting
files. The [test suite](../tests/) checks story validation, raster dimensions,
PDF assembly, and contrast on generated content.
