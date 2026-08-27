# Plan: Connect Contact Form to Netlify Forms

## Overview

The goal is to wire the existing static HTML contact form to **Netlify Forms** so that every submission is captured in the Netlify dashboard and triggers an email notification to the business owner. Because the site is deployed as a static file on Netlify (no server-side code), Netlify Forms is the right fit — it requires only HTML attribute additions and a small JavaScript enhancement for UX. No external third-party service, no backend, no API keys needed.

The scope is intentionally minimal:
1. Annotate the form so Netlify's build bot detects and registers it.
2. Intercept the submit event with JavaScript to POST the data via `fetch`, prevent the default page redirect, and display an inline success (or error) message.

---

## Sub-Tasks

---

### Sub-Task 1 — Register the form with Netlify Forms

**Intent**
Netlify's build bot scans HTML at deploy time for forms carrying `data-netlify="true"`. Adding this attribute (plus a hidden `form-name` field) is what causes Netlify to start capturing submissions and route email alerts to the site owner. Without this step the form POSTs to nowhere.

**Expected Outcomes**
- The `<form>` element carries `netlify` (or `data-netlify="true"`) and `name="book-appointment"`.
- A hidden `<input type="hidden" name="form-name" value="book-appointment" />` exists inside the form so that the JavaScript `fetch` submission is also linked to the correct Netlify form record.
- Netlify's dashboard "Forms" tab will show a `book-appointment` form after the first deploy.

**Todo List**
1. In `index.html`, locate the `<form action="#" method="post">` element (around line 538).
2. Change `action="#"` to `action="/success"` — this serves as a fallback for non-JS browsers but will be overridden by the JS intercept.
3. Add `name="book-appointment"` and `data-netlify="true"` attributes to the `<form>` tag.
4. Insert a hidden input `<input type="hidden" name="form-name" value="book-appointment" />` as the first child of the form.

**Relevant Context**
- File: `index.html`, `<form>` element at line 538.
- Netlify only registers forms it finds in the *built* HTML — no extra config file needed.

**Status** — `[x] done`

---

### Sub-Task 2 — Add inline success/error feedback via JavaScript

**Intent**
By default, Netlify Forms redirects the user to a generic Netlify "success" page after submission. The desired UX is to stay on the same page and show an inline message. This requires intercepting the `submit` event with JavaScript, serialising the form data, and POSTing it to Netlify's form handler endpoint manually via `fetch`. On success, the form is replaced with a confirmation message; on failure, an error message is shown.

**Expected Outcomes**
- Submitting the form does **not** navigate away from the page.
- On successful POST (HTTP 200), the form is hidden and a styled success message appears in its place ("Thank you! We'll be in touch within one business day.").
- On network or server error, a visible inline error message prompts the visitor to try again or call directly.
- The form fields reset after a successful submission.

**Todo List**
1. Find the existing `<script>` block near the bottom of `index.html` (line 568).
2. Add a `DOMContentLoaded` listener that selects the `form[name="contact"]` element.
3. Attach a `submit` event listener that:
   a. Calls `e.preventDefault()`.
   b. Serialises the form using `new FormData(form)`.
   c. Sends a `fetch` POST to `'/'` (Netlify intercepts all POSTs to the same page) with header `'Content-Type': 'application/x-www-form-urlencoded'` and the encoded body (convert `FormData` to URL-encoded string via `URLSearchParams`).
   - Note: the `form-name` hidden field ensures the POST is attributed to the `book-appointment` form record.
   d. On `.then(response => response.ok)`: hide the form, render a success `<p>` element inside the form's parent container.
   e. On error: display an inline error message below the submit button without hiding the form.
4. Add minimal inline styles for the success/error message (use existing CSS variables `--sage-dk` for success, a muted red for error — keep it consistent with the page palette).

**Relevant Context**
- File: `index.html`, `<script>` block near line 568.
- Existing CSS variables: `--sage-dk`, `--muted`, `--text`, `--warm` (defined in `:root` around line 22).
- The form's parent container is `.contact-grid > form` — success message should replace the form visually within that same column.
- Form name used throughout: `book-appointment`.

**Status** — `[x] done`

---

## Notes for Implementation

- Do **not** add a `honeypot` field or reCAPTCHA unless the owner specifically requests spam protection later — keep the change minimal.
- The `action="/success"` fallback is fine as a non-JS graceful degradation path; no `/success` page needs to be created for the JS-enabled flow.
- Netlify email notifications are configured in the Netlify dashboard under **Forms → Settings → Form Notifications** after the first deploy; no code change is needed for that step.
