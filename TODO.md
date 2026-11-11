# template_storybook TODO

This backlog is future-only. Completed validation and dated review evidence are preserved in the
monorepo's `docs/maintenance/exemplar-backlog-history.md`
(not present in this standalone mirror) or in source-owned generated receipts. Each active row must retain a stable ID, size, dependency,
next action, proving artifact, acceptance command, and negative control; absence of an owner or external receipt
keeps a capability blocked rather than silently promoting it.

## Backlog operating rules

- Keep deterministic and offline defaults unchanged unless an upcoming row explicitly scopes an opt-in.
- Do not close a row until its producer, artifact, consumer, gate, and failing negative control are present.
- Treat unavailable network, LLM, container, formal-tool, and publication paths as explicit skips
  or blockers.
- Re-derive counts and receipts from live source data; never copy measurements into this planning file.

## Integrity and template-status gaps

- Keep story text in `content/story.yaml` and rendering behavior in `src/storybook/`.
- Keep every story page as a full-page illustration; no manuscript-style partial
  figures for the primary artifact.
- Page-level accessibility alt text is generated in
  `output/data/storybook_manifest.json`; keep it descriptive when page content
  changes.
- Keep script, test, documentation, and `.agents/` catalog listings generated
  or checked against disk so future forks cannot silently omit a surface.

## Current configurable-surface contract

- `trim_size` supports the current configured and custom page dimensions; a
  future trim variant must add a schema value and a rendering/negative-control
  test in one scoped row.
- Per-page caption placement supports the current top and bottom zones; future
  zones require a schema, accessibility, and raster-contrast contract.

## Documentation and signposting gaps

- Keep README examples clear that the page scripts render the storybook PDF,
  while monorepo Stage 03 renders the descriptive manuscript PDF.
- The deterministic contact sheet is generated at
  `output/figures/storybook_contact_sheet.png` on every full page-script render.

## Test and validator gaps

- Keep a raster contrast audit for direct text overlays, using real pixel math
  and recording per-page results in the manifest; extend the palette contract
  before adding new overlay modes.

## Minor upcoming

| ID | Status | Size | Dependency | Next action / unblock condition | Proving artifact | Acceptance command | Negative control |
| --- | --- | --- | --- | --- | --- | --- | --- |
No active rows are currently scoped at this size.

## Medium upcoming

| ID | Status | Size | Dependency | Next action / unblock condition | Proving artifact | Acceptance command | Negative control |
| --- | --- | --- | --- | --- | --- | --- | --- |
No active rows are currently scoped at this size.

## Major upcoming

| ID | Status | Size | Dependency | Next action / unblock condition | Proving artifact | Acceptance command | Negative control |
| --- | --- | --- | --- | --- | --- | --- | --- |
No active rows are currently scoped at this size.

## Backlog status

Rows remain active until the acceptance command and negative control pass in the same source revision.
A blocked row is a deliberate boundary, not a skipped success.

## Workflowz review — 2026-11-10

- [ ] **TSB-20261110-01 — P1 — Make the direct-text contrast audit measure the rendered artifact.** Size: Medium.
  - **Outcome:** A direct-overlay image with no readable local text/background contrast can no longer pass merely because the declared ink color occurs somewhere in the raster. The full renderer continues to emit a contrast-audit record for every page in `output/data/storybook_manifest.json`.
  - **Evidence:** `src/storybook/text_layout.py:44-47` reads pixels only into a color set; `src/storybook/text_layout.py:53-55` computes contrast from hard-coded ink/shadow constants; `src/storybook/text_layout.py:56-59` gates on global ink membership plus that declared ratio. This does not satisfy the real-raster direct-overlay audit required by `TODO.md:45-47`. A solid `LIGHT_TEXT_RGB` test image with `overlay_box=False` currently passes despite having no contrasting text or background.
  - **Next action:** Derive direct-text and adjacent-background samples from the actual rendered text mask/artwork, calculate local raster contrast, and fail unreadable cases; retain the declared-palette check separately where useful. Add `tests/test_contrast_audit.py::test_direct_overlay_rejects_flat_ink_field`, constructing a uniform `LIGHT_TEXT_RGB` PNG and an `overlay_box=False` `PageSpec`, and asserting that `audit_rendered_text_contrast` raises instead of returning success.
  - **Proving artifact:** The focused regression plus a freshly generated manifest from the complete PDF build.
  - **Acceptance probe (from repository root):** `uv run --extra dev pytest tests/test_contrast_audit.py::test_direct_overlay_rejects_flat_ink_field && uv run python scripts/90_build_storybook_pdf.py`
  - **Negative control / dependencies / constraints:** The unchanged bundled story must build with a per-page audit entry. No dependency is required. Preserve the 4.5 minimum and the manifest's per-page audit surface; if real raster auditing exposes existing page failures, correct the text/art contrast in this row rather than weakening the threshold.

- [ ] **TSB-20261110-02 — P2 — Reject an empty story before creating output artifacts.** Size: Small.
  - **Outcome:** `pages: []` produces a descriptive configuration error from `load_storybook`, before any PDF, manifest, summary, contact sheet, or page image is created or replaced.
  - **Evidence:** `src/storybook/story.py:87-93` accepts an empty list because its numbers equal `range(0)`. `build_storybook_pdf` then obtains no images at `src/storybook/rendering.py:105` but saves the PDF at `src/storybook/rendering.py:110-117`; only the later contact-sheet call at `src/storybook/rendering.py:134` reaches the empty-image error at `src/storybook/rendering.py:67-68`.
  - **Next action:** Add a non-empty-page invariant next to the existing list and contiguous-number checks. Add `tests/test_story.py::test_empty_pages_fail_at_load_before_outputs`: copy `content/` into a temporary project, set `pages` to `[]`, assert a `ValueError` identifying `pages`, and assert that no `output/` directory was created.
  - **Proving artifact:** The focused load-validation regression and a successful unchanged-story build.
  - **Acceptance probe (from repository root):** `uv run --extra dev pytest tests/test_story.py::test_empty_pages_fail_at_load_before_outputs && uv run python scripts/90_build_storybook_pdf.py`
  - **Negative control / dependencies / constraints:** The bundled cover-plus-pages configuration must still load and build with contiguous numbering from page zero. No new dependency or output-layout change is needed.

- [ ] **TSB-20261110-03 — P2 — Validate complete hexadecimal palette colors during story loading.** Size: Small.
  - **Outcome:** Every palette entry is validated as exactly six hexadecimal RGB digits before rendering, and malformed values report the page/palette configuration error rather than a later raw integer-conversion failure.
  - **Evidence:** `src/storybook/story.py:33-40` checks only list length, string type, and a leading `#`; for example `#12zz56` passes. `src/storybook/illustration.py:18-20` subsequently parses slices with base 16, so the invalid configuration fails only during illustration.
  - **Next action:** Tighten `_palette` validation and include page context in its error. Add `tests/test_story.py::test_invalid_hex_palette_fails_at_load`, copying the real story, changing one color to `#12zz56`, and asserting failure from `load_storybook`; include a valid six-digit/mixed-case color as the passing control.
  - **Proving artifact:** The focused schema regression and a successfully rendered existing story page.
  - **Acceptance probe (from repository root):** `uv run --extra dev pytest tests/test_story.py::test_invalid_hex_palette_fails_at_load && uv run python scripts/20_render_page_01.py`
  - **Negative control / dependencies / constraints:** The unchanged four-color story palettes must load and render. Do not add named colors, alpha channels, or new dependencies without separately defining their rendering contract.

- [ ] **TSB-20261110-04 — P2 — Keep configured outputs and manifest paths inside the declared project boundary.** Size: Medium.
  - **Outcome:** Absolute or parent-escaping `output_pdf` values are rejected before writing, and `RenderResult.to_dict(root=...)` fails closed rather than publishing an absolute checkout path for a result outside that root.
  - **Evidence:** `src/storybook/story.py:95` accepts any path text, while `src/storybook/rendering.py:106-107` joins it to the root and creates its parent; an absolute path discards the root and `..` can escape it. This conflicts with the project-relative path statement at `README.md:83`. Independently, the serializer promises that a checkout path never becomes publication metadata at `src/storybook/models.py:85-90`, but its outside-root fallback at `src/storybook/models.py:92-98` returns `str(path)` unchanged.
  - **Next action:** Enforce a relative, non-escaping output-PDF path during loading and make rooted manifest serialization reject outside-root paths explicitly. Add `tests/test_story.py::test_output_pdf_cannot_escape_project_root` for both `../escaped.pdf` and an absolute target, asserting no target is created. Add `tests/test_rendering.py::test_manifest_paths_fail_closed_outside_root`, exercising `RenderResult.to_dict(root=...)` with an outside absolute result and an inside-root passing control.
  - **Proving artifact:** Both behavioral regressions and the normal manifest generated by a complete build.
  - **Acceptance probe (from repository root):** `uv run --extra dev pytest tests/test_story.py::test_output_pdf_cannot_escape_project_root tests/test_rendering.py::test_manifest_paths_fail_closed_outside_root && uv run python scripts/90_build_storybook_pdf.py`
  - **Negative control / dependencies / constraints:** Existing relative outputs and the scripts' `--project-root` override must continue working, and normal manifest paths must remain project-relative. Resolve containment without requiring the destination file to exist; no network, dependency, or board prerequisite applies.

- [ ] **TSB-20261110-05 — P3 — Reject misspelled or undeclared trim-size labels.** Size: Small.
  - **Outcome:** Only `custom` and keys declared in `TRIM_SIZES` load successfully; named sizes retain their dimensions check, while a typo fails at configuration time instead of silently behaving as custom metadata.
  - **Evidence:** `src/storybook/story.py:98-103` validates any non-empty string but checks dimensions only when the value happens to occur in `TRIM_SIZES` (`src/storybook/story.py:44-48`). Thus `portriat` bypasses the portrait contract. `TODO.md:30-32` explicitly requires a future trim variant to add a schema value and its rendering/negative-control coverage.
  - **Next action:** Add an explicit allowed-label check while retaining arbitrary positive dimensions for `custom`. Add `tests/test_story.py::test_unknown_trim_size_rejected` using a copied story with `trim_size: portriat`, and `tests/test_story.py::test_named_and_custom_trim_sizes_remain_valid` to exercise one matching named geometry and one custom geometry.
  - **Proving artifact:** The two focused schema regressions and a normal page render using the repository configuration.
  - **Acceptance probe (from repository root):** `uv run --extra dev pytest tests/test_story.py::test_unknown_trim_size_rejected tests/test_story.py::test_named_and_custom_trim_sizes_remain_valid && uv run python scripts/20_render_page_01.py`
  - **Negative control / dependencies / constraints:** Preserve the bundled story's configured dimensions and both current operating modes (`custom` and declared named sizes). This is validation only: no new trim geometry, raster redesign, or dependency upgrade is in scope.
