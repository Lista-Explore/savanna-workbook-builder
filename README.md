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

- The Builder is three views — **Build**, **Live Preview**, **Publish** —
  not one long page. Build opens on a compact outline of your worksheets;
  clicking one opens an editor scoped to just that worksheet, so you're
  never looking at your whole workbook's content at once.
- On a fresh session, Build opens with an already-editable blank workbook
  plus a small inline prompt offering to start from the example or import
  an existing workbook instead — not a gate, just an option sitting above
  content you can start typing into immediately. Returning sessions with a
  saved draft skip it and resume straight into your own work.
- A section is an ordered list you fully control: headings, free-text blocks,
  images, and questions can be mixed in any order, not just questions.
- Sections can be collapsible in the student view, and can use 1, 2 or 3
  columns; any item can be assigned to a column, and the Builder lays items
  out in real side-by-side columns as you edit — not just in Live Preview —
  so you can see the layout you're actually building.
- Every worksheet, section, and item can be moved up/down, duplicated, or
  deleted (with a confirmation) directly in the Builder.
- A "Load example workbook" button gives you a working starting point that
  already matches the sample PDF included in this repo.
- The Builder autosaves your in-progress workbook definition to this browser
  on every edit and restores it if you reload or reopen the tab.
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

## Images and text blocks

An image is just another item in a section, positioned wherever you place it
relative to your questions. The builder stores an image URL; in the LMS HTML
this becomes a normal `<img>`. For PDF generation, the builder fetches and
embeds the image — the URL must allow cross-origin access for embedding to
succeed. If it can't be embedded, the PDF shows a labelled placeholder
instead of failing. Heading and text-block items are read-only content (no
answer to save), so they never appear as PDF form fields.

## Local development

Serve this directory over HTTP (loading `studio.html` via `file://` will not
work because of CORS restrictions on the CDN scripts) and open `studio.html`
or `index.html` — both are equivalent, complete entry points:

```
python3 -m http.server 8080
```

See `CHANGELOG.md` for what changed recently, `TESTING.md` for the manual
acceptance-test checklist, and `ENGINEERING_HANDOFF.md` for integration notes.
