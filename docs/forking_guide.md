# Forking guide

Start with a clean copy of this standalone repository, or use the
[copy helper](https://github.com/docxology/template/blob/87704feae0a4490ef7dad9c931d268e8c30273c2/scripts/audit/copy_exemplar.py)
from a shared template checkout containing the exemplar.

1. Replace [`content/story.yaml`](../content/story.yaml) with your own
   story, cast, pages, captions, and meaningful alt text.
2. When the existing scene types cannot express the new story, add scene
   branches in `src/storybook/illustration.py`. Keep typography and
   overlays in `src/storybook/text_layout.py`.
3. Update [`docs/manuscript/config.yaml`](manuscript/config.yaml) from its
   [example](manuscript/config.yaml.example), including the sequence of
   analysis scripts. Update the root citation sidecars to identify your
   own work. Record publication identifiers after they are assigned.
4. Run the [locked tests and book build](quickstart.md). Inspect every
   page and compare the contact sheet and manifest with the source order.
   Refresh the descriptive manuscript captions and figures for your fork.
