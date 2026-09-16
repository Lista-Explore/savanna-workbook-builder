# Changelog

## v1.5.2 — 2026-09-16

- Fixed: a new worksheet auto-created a section, and a new section
  auto-created a "Question" field — and that field's Delete button was
  disabled while it was the only one, so it was effectively stuck there
  unless you added a second item first. Worksheets and sections now start
  empty; you add exactly what you want via the buttons, and any item can
  always be deleted, down to zero if you want.
- Removed the per-item "Column" dropdown. Moving an item between columns
  is now explicit buttons ("→ Col 2", "→ Col 3") next to the other item
  actions, matching the per-column add buttons already there.
- Sections had no visual boundary after the last density pass — just a
  thin top rule, so with more than one section it wasn't clear where one
  ended and the next began. Sections are now a distinct light-toned
  container (lighter than the worksheet card, so it doesn't recreate the
  nested-box problem), giving a real, visible edge.
- Section collapse/expand now toggles by clicking anywhere on the section
  header banner (with a chevron indicating state), not just a small
  button — a much bigger, easier target. Clicking the actual action
  buttons (move/duplicate/delete) in that same header no longer also
  triggers collapse.
- Fixed a layout bug this surfaced: with the new column-move buttons
  added, a question's action row (up to 6 buttons) could overflow a
  narrow multi-column bucket and visually overlap the next column. The
  header row now wraps properly above a certain button count.

## v1.5.1 — 2026-09-16

Added a lightweight starting choice, but not as a gate. The Build outline
always opens straight into an already-editable blank workbook — nothing
blocks you from just typing a title and going. On a fresh session (no saved
draft), a small inline banner sits above it: *"Starting a new workbook? ...
or start differently"* with two buttons, **Use the example instead** and
**Import existing workbook instead**. Picking one swaps the content in
place (or opens a file picker for import) without navigating away from the
page; dismissing it or just editing the blank workbook makes it go away.
Returning sessions with a saved draft skip it entirely and resume straight
into their own work, as before.

## v1.5 — 2026-09-16

Restructured the Builder around the actual content model instead of putting
the entire workbook on one continuously-scrolling page. Previously, opening
the Builder rendered workbook settings, every worksheet, every section,
every question, Live Preview, and Publish all at once — for anything beyond
a trivial workbook this was overwhelming regardless of how individual boxes
were styled, because the problem was structural, not visual.

- The Builder now has three views, reachable from tabs at the top:
  **Build**, **Live Preview**, **Publish** — each full width, one thing at
  a time, instead of three panels stacked on the same page.
- **Build** opens on a compact outline: workbook settings plus a list of
  worksheets (title and a "3 sections · 8 questions" count each), not
  their expanded content. Click a worksheet to open an editor scoped to
  *only* that worksheet — other worksheets aren't in the DOM at all while
  you're working on one. A "← All worksheets" link goes back.
- Reordering, duplicating, and deleting worksheets now happens from the
  outline list; deleting a worksheet you're currently editing returns you
  to the outline afterward.
- Worksheet-level collapse/expand no longer exists as a separate control,
  because it's no longer needed — you only ever see one worksheet's
  content at a time. Section-level collapse (for organizing a long
  worksheet) is unchanged.
- Fixed a bug this restructure surfaced: the "restored your draft"
  message was being written into the Publish panel's output area, which
  no longer exists by default on load (Build does). Moved it to a
  one-time banner on the outline screen.
- Removed the sidebar and its non-functional click targets entirely — the
  outline list now serves that purpose and actually works.

## v1.4 — 2026-09-16

- Reverted the sticky 3-column split (editor / preview side-by-side) —
  it squeezed everything into narrow columns and made the page feel more
  cramped, not less. Back to a single, full-width editing column.
- Fixed the actual source of the "squeezed" feeling: worksheet, section,
  and question were each wrapped in their own bordered, shadowed white
  box, so editing a question meant looking at a box inside a box inside
  a box, all the same visual weight. Only the worksheet card keeps that
  treatment now; sections and questions inside it are told apart with
  spacing, dividers, and type size instead of repeated boxes.
- The left sidebar listed "Workbook settings" and each worksheet like a
  table of contents, but clicking them did nothing — no cursor change, no
  handler. They're now real navigation: clicking a worksheet scrolls to it
  and expands it first if it was collapsed.
- Simplified the question editor: every question showed Help, Placeholder,
  Pattern, and a 5-field validation grid regardless of whether any of it
  applied — a Yes/No checkbox showed the same form as a Number field. Those
  are now tucked behind a closed "Advanced options" disclosure, cutting each
  question card roughly in half by default. It opens automatically if a
  question already has any of that filled in, so nothing already configured
  gets hidden.
- Fixed a significant disorientation bug: Live Preview reset to worksheet 1
  on every single edit anywhere in the Builder, even something unrelated on
  a different worksheet. If you were reviewing worksheet 3 and tweaked a
  label on worksheet 1, Preview would yank you back to worksheet 1. It now
  stays on whichever worksheet tab you were viewing.
- Fixed three buttons that didn't work as expected, found by clicking every
  control in the Builder rather than assuming they worked:
  - Delete (on a worksheet, section, or question) silently did nothing when
    it was the last one, with zero feedback — no dialog, no message. It's
    now visibly disabled with a tooltip explaining why, instead of a no-op.
  - "Copy HTML" could throw an uncaught clipboard error and gave no
    indication of success or failure either way. It now falls back to a
    legacy copy method if the modern API is unavailable, and always shows
    "Copied!" or a clear failure message.
  - Deleting a dropdown/radio/checklist's options had no floor — you could
    delete down to zero options, leaving a broken empty field with no
    warning. The last option's Delete button is now disabled.
- Fixed: every column had its own visual bucket in the Builder, but there was
  only one "+ Add content" button for the whole section, so a new item
  always landed in Column 1 regardless of which column you were looking at,
  and it always defaulted to a generic question with no obvious way to make
  it a heading/text/image without a follow-up edit. Each column now has its
  own add row with explicit "+ Question / + Heading / + Text / + Image"
  buttons, so new items land exactly where you're adding them, as the type
  you actually meant.
- Removed a duplicated ~35-line block of authoring-UI CSS (an entire earlier
  copy of the same rules, always overridden by a near-identical later one)
  and about a dozen CSS classes left over from an earlier design draft that
  the generated HTML never used (`.question-card`, `.studio-form-field`,
  `.preview-shell`, `.danger-button`, and others). No visual change; this
  was a cleanliness pass, verified with a script that checks every CSS class
  is actually referenced somewhere in the generated output.
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
