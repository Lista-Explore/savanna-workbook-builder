# Manual test checklist

Use this list to verify a new build before publishing. Serve the folder over
HTTP and open `studio.html`.

## Builder / Live Preview

- [ ] Add a second and third worksheet — every tab is clickable and shows the
      correct worksheet.
- [ ] Add a question of each type (short text, long text, number, dropdown,
      radio, checkbox, checkbox group, option list, date) and confirm it
      renders correctly in Live Preview.
- [ ] Set a section to 2 or 3 columns and confirm the layout updates.
- [ ] Enable "Allow students to collapse this section" and confirm the
      collapse/expand toggle works.
- [ ] Add a section image and confirm it renders in the assigned column.

## PDF

- [ ] Fill in a few answers in Live Preview and click "Download PDF" — open
      the file in a PDF reader and confirm every field is actually clickable
      and editable, not just a drawn box.
- [ ] Reset the preview, then upload the PDF you just downloaded — confirm
      the answers repopulate into the form.
- [ ] Upload `sample_partially_filled_progress.pdf` (included in this repo)
      against a workbook shaped like the original "Customer Analysis"
      example — confirm its answered fields import correctly. This checks
      backward compatibility with older PDFs.

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
