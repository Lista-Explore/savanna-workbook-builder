# Manual test checklist

Use this list to verify a new build before publishing. Serve the folder over
HTTP and open `studio.html`.

## Builder / Live Preview

- [ ] Make an edit (e.g. change the workbook title), reload the page, and
      confirm the edit is still there with a "Restored your unsaved draft"
      message — this is the Builder's own autosave, separate from student
      answer storage.
- [ ] Add a second and third worksheet — every tab is clickable and shows the
      correct worksheet.
- [ ] Add a question of each type (short text, long text, number, dropdown,
      radio, checkbox, checkbox group, option list, date) and confirm it
      renders correctly in Live Preview.
- [ ] Set a section to 2 or 3 columns and confirm the Builder itself (not
      just Live Preview) immediately shows real side-by-side columns.
      Change an item's Column dropdown and confirm it jumps to the correct
      column bucket right away. Confirm an item's Up/Down buttons move it
      within its own column and don't jump it into another column.
- [ ] Enable "Allow students to collapse this section" and confirm the
      collapse/expand toggle works (this is the student-facing collapse; see
      below for the separate Builder-authoring collapse).
- [ ] Add a heading, a text block, and an image to the same section, in
      between two questions, and confirm they render in that exact order in
      Live Preview.
- [ ] Move a question up/down within a section and confirm both the Builder
      card order and Live Preview order update to match.
- [ ] Duplicate a worksheet, a section, and a question, and confirm the copy
      appears right after the original with independent state (editing the
      copy doesn't affect the original).
- [ ] Click "Collapse" on a worksheet or section card in the Builder, then
      make an unrelated edit elsewhere (e.g. type in the workbook title) —
      confirm the card stays collapsed.
- [ ] Click "Delete" on a worksheet/section/question and cancel the
      confirmation — confirm nothing is deleted. Confirm it and check it is.
- [ ] Click "Load example workbook", confirm it loads without errors, and
      confirm its content-block intro renders in Live Preview.
- [ ] Generate the LMS HTML, download it, then use "Import existing LMS
      HTML" to load it back in — confirm headings, text blocks, images, and
      questions all come back in the same order and same columns.

## PDF

- [ ] Fill in a few answers in Live Preview and click "Download PDF" — open
      the file in a PDF reader and confirm every field is actually clickable
      and editable, not just a drawn box.
- [ ] Reset the preview, then upload the PDF you just downloaded — confirm
      the answers repopulate into the form.
- [ ] Click "Load example workbook", then in Live Preview upload
      `sample_partially_filled_progress.pdf` (included in this repo) —
      confirm 5 answered fields import correctly. This is the backward-
      compatibility check for older PDFs, and also the check that a fresh
      user's first PDF upload actually shows a visible result.
- [ ] Upload the same PDF against a *blank* (non-example) workbook and
      confirm the "no fields matched" message is clearly visible, not silent.

## Static checks

- `node --check workbook-studio.js`
- `node --check lms-workbook.js`
- `auto-test.html` — open it directly; all assertions listed in
  `#test-result` should read `true`.

## Known limitation

`<input type="file">` fields cannot be restored from a saved PDF or from
browser storage — this is a browser restriction, not specific to this tool.
Treat file-upload answers as not portable between devices via the PDF save
file.
