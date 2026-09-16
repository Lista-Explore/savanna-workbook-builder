# Engineering handoff — LMS Workbook Builder + Runtime

## Architecture

There are two separate products:

1. Workbook Builder: standalone authoring application for instructional designers.
2. LMS Runtime: shared external CSS/JS loaded by the LMS. Generated workbook content is an HTML element only.

The Builder generates both the HTML element and the matching fillable PDF from one workbook definition.

## Generated LMS HTML

The generated output must remain a normal content element. It must not include `<script>`, `<link>`, CDN references, runtime configuration objects, or JavaScript blocks. Shared runtime CSS/JS are hosted externally.

Every non-empty line in the generated HTML contains code. There are no blank lines.

## Runtime pairing

Each HTML field has a stable `data-field-id` and `data-pdf-name`.

PDF download fills the already-published PDF asset using those names.

PDF upload never reconstructs the workbook. It reads answers from the uploaded PDF and applies them to the already-rendered HTML fields. It then persists the merged data to IndexedDB first, localStorage second, memory last.

Merge rule:

- answered uploaded fields overwrite the corresponding current HTML answer;
- blank/unanswered uploaded fields are ignored;
- existing current answers therefore survive an older/partial PDF upload.

## Backwards compatibility

Import must recognise current stable names plus legacy names used by earlier prototype PDFs, including:

- `Worksheet N::Section::Field`
- `Worksheet N::Section::long::Field`
- `Worksheet N::Section::date::Field`
- `Worksheet N::Section::text::Field`
- `Worksheet N::Section::needs::Field`

Matching should prefer exact stable names, then exact legacy names, then unique normalised matches. Do not use an ambiguous label match when multiple PDF fields could match.

## Sections

Section properties:

- title
- instructions/description
- columns: 1, 2 or 3
- collapsible: true/false
- defaultOpen: true/false
- fields: an ordered list of items (see below)

A section's `fields` array is a single ordered list mixing content and
questions — there is no separate list for images. Each item has a `type` and
a `column`. Three types are content-only, never form fields, and never carry
a `data-field-id` or `data-pdf-name`:

- `heading` — short bold text, uses the item's `label` as the heading text.
- `text` — a free-text/instructions block, uses `label` as the body text
  (rendered with `white-space:pre-wrap`, so newlines in the Builder's textarea
  are preserved as line breaks).
- `image` — has `url`, `alt`, `width` in addition to `column`; skipped
  entirely (both in HTML and PDF output) when `url` is empty.

Every other `type` is a real question and works as before. Because content
items never get a `data-field-id`, the runtime's field-scanning selector
(`[data-field-id]`) automatically excludes them from `getData`/`applyData`/
`capture`/validation with no special-casing required there.

The online runtime uses CSS Grid. On small screens, multi-column sections collapse to one column.

## PDF

Short text and long text must be real AcroForm text fields added to the page. Long text must call `enableMultiline()` before `addToPage()`.

Dropdowns, option lists, radio groups and checkboxes must be actual AcroForm controls.

PDF sections are always expanded; online collapse is an LMS navigation feature only.

Images are embedded when their URLs are CORS-readable. If an image cannot be fetched/embedded, the PDF should show a clear placeholder and continue rather than fail the entire document.

## Storage

No LMS answer storage is required.

Primary: IndexedDB.
Fallback: localStorage.
Last fallback: in-memory state.
Portable backup: downloaded fillable PDF.

## Production dependencies

For production, self-host/pin pdf-lib, fontkit and Poppins rather than relying on public CDNs.
