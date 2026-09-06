# Changelog

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
