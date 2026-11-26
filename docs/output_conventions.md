# Output conventions

| Output | Role |
| --- | --- |
| `output/figures/storybook_pages/*.png` | One image for each story or cover page |
| `output/figures/storybook_contact_sheet.png` | Ordered page thumbnails for inspection |
| `output/pdf/the-shape-between.pdf` | Illustrated book assembled by ReportLab |
| `output/data/storybook_manifest.json` | Page inventory, alt text, paths, and raster contrast checks |
| `output/reports/storybook_summary.md` | Book build summary |
| `output/pdf/template_storybook_combined.pdf` | Shared template's combined descriptive manuscript |
| `output/web/index.html` and `output/slides/` | Descriptive manuscript HTML and slides |
| `output/reports/validation_report.md` | Shared template validation report |

Repository copies under `output/` document a prior release. Use the
[book builder](quickstart.md) and [manuscript render](manuscript/README.md)
to produce outputs for the current source in a disposable checkout.

ReportLab uses the image filenames as part of the PDF resource identifiers.
A fresh build from another checkout path can therefore change PDF bytes
even when its page PNGs, contact sheet, and manifest match.
