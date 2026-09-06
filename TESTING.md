# Test record

## 2026-09-06 — real browser verification

Previous test records in this file's history were written without ever
running the app in a browser, which let a real bug (`renderPreview()` called
everywhere but never defined) ship across several versions undetected. The
following was verified against a live instance of `studio.html`, not just
read from source:

- Added a second worksheet in the Builder — both tabs render with correct
  `data-workbook-tab-index`, and clicking each one shows the right worksheet
  and hides the other.
- Filled a short-text field, clicked "Download PDF" in Live Preview, loaded
  the resulting bytes back through pdf-lib: a real `PDFTextField` exists with
  the typed value.
- Built a throwaway model covering all field types (short-text, dropdown,
  radio, checkbox, checkbox-group, option-list) and confirmed `generatePDF`
  creates a `PDFTextField` / `PDFDropdown` / `PDFRadioGroup` / `PDFCheckBox` /
  `PDFOptionList` respectively — real fillable controls, not drawn boxes.
- Reset the preview form, re-uploaded the downloaded PDF, and confirmed the
  field value repopulated in the live HTML and the status line reported the
  import.
- Loaded the actual `sample_partially_filled_progress.pdf` shipped in this
  repo (legacy `Worksheet N::Section::Field` naming) against a model shaped
  to match its original schema, and confirmed all 5 answered fields (Customer
  Name, Customer Type, Review Date, the two checked needs, Main Customer
  Problem) imported correctly; the two unanswered legacy fields were correctly
  left blank rather than overwritten.
- Enabled "Allow students to collapse this section" and confirmed the toggle
  button collapses/expands `.lms-workbook-section-body` in the live preview.
- Set a section to 2 columns and confirmed `data-columns="2"` and the
  corresponding CSS grid are applied in the rendered preview.
- Ran `auto-test.html` (a self-check harness that existed in this repo but had
  never actually completed a run due to a `null.click()` bug) to completion;
  all 8 assertions pass: builder-styled, two-tabs, second-tab-visible,
  collapse-control, collapse-works, columns-present, runtime-mounted,
  legacy-pdf-import-matches.

## Static checks (carried over, still valid)

- `node --check workbook-studio.js` — PASS
- `node --check lms-workbook.js` — PASS
- No blank lines in generated HTML — PASS

## Known limitation

`<input type="file">` fields cannot be restored from a saved PDF or from
browser storage — this is a browser security restriction, not a bug in this
tool. If a workbook needs file uploads, treat that answer as not portable
between devices via the PDF save file.
