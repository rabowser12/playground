# Calendly Scheduling Integration Plan

## Overview

Integrate a Calendly inline booking widget into the existing `index.html` therapy practice site so that visitors can self-schedule sessions without leaving the page. The integration must visually match the existing warm, calm design system and be structured so that swapping Calendly out for SimplePractice in the future requires only a targeted change to a single section.

**Scope:** Changes are limited to `index.html` only. No new files are created.

**Out of scope:** Calendly account setup, event type configuration, and connecting Calendly to a calendar provider — these are done in the Calendly dashboard by the practice owner before the embed code is placed.

---

## Sub-Tasks

---

### Sub-Task 1 — Add Calendly Script and CSS to the `<head>`

**Intent**
Calendly's inline widget requires two external assets loaded in the `<head>`: a CSS file and a JS file. Adding these once in the `<head>` makes them available to the widget anywhere on the page. Isolating this as its own step keeps the change minimal and reviewable.

**Expected Outcomes**
- The Calendly widget CSS (`https://assets.calendly.com/assets/external/widget.css`) is linked in the `<head>`.
- The Calendly widget JS (`https://assets.calendly.com/assets/external/widget.js`) is loaded via a `<script>` tag in the `<head>` (or just before `</body>` — either works; bottom of `<body>` is preferred for performance).
- No visual change to the page yet — assets are loaded but nothing is rendered.

**Todo List**
1. Add a `<link rel="stylesheet">` tag for the Calendly CSS inside the `<head>`, after the existing `<style>` block.
2. Add a `<script src="...">` tag for the Calendly JS just before the closing `</body>` tag (alongside the existing inline `<script>` block).

**Relevant Context**
- `index.html` line 357: closing `</style>` tag — insert the `<link>` after this line.
- `index.html` line 589–628: existing inline `<script>` block — add the Calendly JS `<script>` tag just before or after this block, before `</body>`.

**Status:** [x] done

---

### Sub-Task 2 — Add the Calendly Inline Widget to the `#contact` Section

**Intent**
Place a Calendly inline widget `<div>` at the top of the right column (the `<form>` column) in the `#contact` section, stacked above the existing contact form. A visual divider between the widget and the form clarifies that they serve two distinct purposes: the widget is for direct booking, the form is for general inquiries.

**Expected Outcomes**
- A `<div class="calendly-inline-widget">` element with a placeholder `data-url` attribute appears above the `<form>` in the right column of `.contact-grid`.
- The widget renders at a sensible fixed height (e.g. `630px`) that matches Calendly's default inline widget dimensions.
- A clear label or heading above the widget (e.g. "Book a Session") distinguishes it from the "Send a Message" form below.
- A subtle visual separator (e.g. a horizontal rule or spacing + label) appears between the widget and the form.
- The `data-url` attribute contains a clearly marked placeholder value (`https://calendly.com/YOUR_USERNAME/YOUR_EVENT`) so the practice owner knows exactly what to replace.

**Todo List**
1. Above the `<form>` element (line 558), insert a heading label "Book a Session" styled with the existing `.section-label` class or inline styles consistent with the design system.
2. Insert the Calendly `<div>` element with `class="calendly-inline-widget"`, `data-url="https://calendly.com/YOUR_USERNAME/YOUR_EVENT"`, and `style="min-width:320px;height:630px;"`.
3. Add a visual divider (e.g. `<hr>` with appropriate spacing, or a styled `<p>` label "Or send us a message") between the Calendly widget and the existing `<form>`.

**Relevant Context**
- `index.html` lines 558–578: the `<form>` element that occupies the right column of `.contact-grid`.
- Calendly's standard inline embed markup: `<div class="calendly-inline-widget" data-url="..." style="min-width:320px;height:630px;"></div>`
- The existing design tokens (`--sage`, `--sage-dk`, `--warm`, `--muted`, `--border`) from `:root` at line 38 should be used for any new styling.

**Status:** [x] done

---

### Sub-Task 3 — Update "Schedule an Appointment" Buttons to Target the Calendly Widget

**Intent**
The nav CTA and the hero CTA both currently `href="#contact"`, which scrolls to the top of the contact section. After adding the Calendly widget, these should scroll directly to the widget so visitors land on the booking tool immediately. This requires adding an `id` to the Calendly widget wrapper and updating two anchor `href` attributes.

**Expected Outcomes**
- The Calendly widget `<div>` (or its wrapper) has `id="book-session"`.
- The nav "Schedule an Appointment" button `href` is updated from `#contact` to `#book-session`.
- The hero "Schedule an Appointment" button `href` is updated from `#contact` to `#book-session`.
- Both buttons' `aria-label` attributes are updated to reflect the new target ("jump to booking calendar").
- Clicking either button smoothly scrolls to and focuses the booking widget.

**Todo List**
1. Add `id="book-session"` to the wrapper element created in Sub-Task 2.
2. Update `href="#contact"` → `href="#book-session"` on the nav CTA (line 373).
3. Update `href="#contact"` → `href="#book-session"` on the hero CTA (line 383).
4. Update both `aria-label` values to reflect the new destination.

**Relevant Context**
- `index.html` line 373: nav "Schedule an Appointment" anchor.
- `index.html` line 383: hero "Schedule an Appointment" anchor.
- Sub-Task 2 creates the target element — this sub-task must run after Sub-Task 2 is complete.

**Status:** [x] done

---

### Sub-Task 4 — Add Responsive Styles for the Calendly Widget

**Intent**
Calendly's inline widget uses a fixed height by default. On mobile (below 820px breakpoint already used in the site), the widget needs to be full-width and have an appropriate height so it doesn't feel cramped or overflow. This also ensures the stacked layout (widget above form) looks correct at all viewport sizes.

**Expected Outcomes**
- On screens wider than 820px, the widget takes the full width of the right column with `height: 630px`.
- On screens 820px and below, the widget is full-width with a slightly reduced height (e.g. `height: 580px`) that still gives Calendly enough room to render without scrolling issues.
- No layout breakage to the existing `.contact-grid` responsive collapse.

**Todo List**
1. Inside the existing `@media (max-width: 820px)` block (line 350–356), add a rule for `.calendly-inline-widget` setting an appropriate responsive height.
2. Optionally add a CSS comment marking the Calendly widget styles for easy identification when replacing with SimplePractice later.

**Relevant Context**
- `index.html` lines 350–356: existing responsive media query block.
- Calendly's widget renders in an `<iframe>` internally — it handles its own internal responsiveness; we only need to control the outer container height.

**Status:** [x] done

---

## Future Migration Note

When switching from Calendly to SimplePractice:
- Remove the two Calendly `<link>` and `<script>` asset tags added in Sub-Task 1.
- Replace the `<div class="calendly-inline-widget" data-url="...">` with the SimplePractice embed markup.
- The `id="book-session"` wrapper, button hrefs, and responsive CSS rules from Sub-Tasks 3 and 4 can remain unchanged.
