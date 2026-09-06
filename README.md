# LMS Workbook Builder + Runtime

A configuration-driven workbook component for instructional designers: build a
workbook (worksheets → sections → fields) in a visual authoring tool, and get
back a plain HTML snippet plus a matching fillable PDF. No HTML/JS knowledge
required from the person authoring content.

This package has two separate concerns:

- **Workbook Studio** (`studio.html` / `index.html` + `workbook-studio.js`):
  the authoring tool instructional designers use to build a workbook, preview
  it live, and generate the HTML + PDF to publish.
- **LMS Runtime** (`lms-workbook.js` + `lms-workbook.css`): the small shared
  script/stylesheet that runs the generated HTML inside the actual LMS — tabs,
  validation, autosave, PDF import/export.

The generated LMS content is HTML-only: no `<script>` or `<link>` tags are
emitted into the published snippet. The runtime script/stylesheet are hosted
once and shared across every workbook.

## Features

- Sections can be collapsible in the student view.
- Sections can use 1, 2 or 3 columns; questions and images can be assigned to a column.
- The generated HTML renders the real form controls, not placeholders.
- Worksheet tabs use explicit worksheet indexes, so any number of worksheets works correctly.
- Styling is Poppins, grayscale/black-and-white, with no brand colour — intended to be restyled per LMS.

## Student storage

IndexedDB is the primary browser store, with `localStorage` as a fallback and
in-memory storage as a last resort. No server-side answer database is
required. The downloaded fillable PDF is the portable save file for moving
work between browsers or devices — uploading it back in merges any answered
fields into the existing form without erasing current answers for fields the
PDF left blank.

## PDF fields

Every question type maps to a real, fillable AcroForm field (text, dropdown,
radio group, checkbox, checklist, or multi-select list) generated in the
browser with [pdf-lib](https://pdf-lib.js.org/), not just a drawn box. Fields
are matched between the HTML and the PDF by a stable generated ID, with
fallback matching against legacy naming schemes from earlier versions of this
tool so older student PDFs keep importing correctly after the workbook is
edited.

## Images

The builder stores an image URL. In the LMS HTML this becomes a normal
`<img>`. For PDF generation, the builder fetches and embeds the image — the
URL must allow cross-origin access for embedding to succeed. If it can't be
embedded, the PDF shows a labelled placeholder instead of failing.

## Local development

Serve this directory over HTTP (loading `studio.html` via `file://` will not
work because of CORS restrictions on the CDN scripts) and open `studio.html`
or `index.html` — both are equivalent, complete entry points:

```
python3 -m http.server 8080
```

See `CHANGELOG.md` for what changed recently, `TESTING.md` for the manual
acceptance-test checklist, and `ENGINEERING_HANDOFF.md` for integration notes.
