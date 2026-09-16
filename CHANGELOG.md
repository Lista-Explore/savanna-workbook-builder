# Changelog

## v1.4 — 2026-09-16

- Multi-column sections are now laid out as real side-by-side columns in the
  Builder itself, not just in Live Preview. Changing an item's column
  reflects immediately, and moving an item up/down moves it within its own
  column instead of jumping across columns — the goal being that you can see
  and trust the layout while you design, not just guess from a dropdown.
- Fixed a real data-loss risk: the Builder never saved the workbook you were
  authoring anywhere — only a student's answers were saved, not your
  questions/sections/worksheets. An accidental refresh or closed tab lost
  everything you had built. The Builder now autosaves your draft to this
  browser on every edit and restores it automatically next time you open it.
- Sections are now a single ordered list of items, and a "question" is just one
  kind of item. You can freely mix in headings and free-text blocks between
  questions, and images can now be placed anywhere in that order too (previously
  images always rendered before every question in a column).
- Every worksheet, section, and question/content item in the Builder can be
  moved up, moved down, or duplicated — no more rebuilding similar content
  from scratch or being stuck with the order you first added things in.
- Worksheet and section cards in the Builder can be collapsed while you work,
  independent of the student-facing collapsible-section feature.
- Deleting a worksheet, section, or question now asks for confirmation first.
- Added a "Load example workbook" button that loads a small pre-built workbook
  whose questions already match the sample PDF included in this repo, so you
  can see the upload/download PDF flow actually work without building
  anything first.
- The status message shown after a PDF upload (including "no fields matched")
  is now a visible bordered banner instead of small grey text easy to miss.
- Fixed: a PDF upload that matched zero questions gave no visible feedback
  strong enough to be noticed. The example workbook above gives a positive
  case to compare against, and the banner styling makes both outcomes clear.

## v1.3 — 2026-09-06

- Fixed the Live Preview panel in the Builder not rendering.
- Fixed worksheet tabs beyond the first not being clickable.
- Fixed short/long text fields in the generated PDF not being fillable.
- Fixed uploaded PDF answers not populating back into the workbook form.
- `index.html` and `studio.html` are now equivalent, complete entry points.
- Fixed a hang in the internal QA test page.
- Removed two outdated example files that no longer matched the current runtime.
- Consolidated duplicate per-version documentation into single files.

## v1.2

- Added collapsible sections with a configurable default-open state.
- Added 1/2/3-column section layouts, with per-question and per-image column assignment.
- Added section image blocks (URL, alt text, width, column).
- Matching HTML and PDF layout support for columns, collapsing, and images.
- PDF field matching supports both current stable field IDs and legacy field names.
- Long and short text PDF fields are real, fillable form fields; long text is multiline.

## v1.1

- Generated PDF text-backed fields (short text, long text, date, number, etc.) are real fillable form fields placed over the designed response area.
- Long text PDF fields are multiline.
- Worksheet tabs carry an explicit index so any number of worksheets works correctly.
- Preview uses an isolated storage namespace, separate from live student data.
- PDF import persists merged answers through the same storage path as regular autosave.
- PDF field matching is tolerant of legacy naming from earlier versions.

## v1.0

- Restored full styling for the Workbook Builder authoring interface.
- Backwards-compatible PDF import for legacy field naming (text, date, dropdown, radio, checkbox-group).
- PDF import distinguishes an unanswered PDF from one belonging to a different workbook.

## v0.9

- PDF import supports both current field IDs and legacy semantic field names.
- Upload only merges answered PDF fields; blank fields never erase existing answers.
- Checkbox groups, radio groups, dropdowns, option lists and text fields all import correctly.
- Generated LMS HTML is HTML-only, with no embedded script or link tags.
