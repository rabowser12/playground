# Practice Name — Therapy & Counseling Website

A single-page static website for a private therapy and counseling practice. Built with plain HTML and CSS — no frameworks, no build step, no dependencies.

---

## Live Site

Hosted on GitHub Pages: `https://rbowser12.github.io/playground/`

---

## Features

- **Responsive layout** — adapts to mobile, tablet, and desktop
- **Smooth scroll navigation** — fixed nav bar with anchor links to each section
- **Calendly inline booking** — embedded scheduling widget lets visitors self-book sessions directly on the page; powered by [Calendly](https://calendly.com)
- **Contact form** — integrated with [Formspree](https://formspree.io) for email delivery; inline success/error feedback via JavaScript (no page redirect)
- **FAQ accordion** — native `<details>`/`<summary>` elements, no JavaScript required
- **Accessibility** — WCAG-compliant markup including skip link, ARIA landmark labels, `aria-hidden` decorative icons, and screen-reader-announced form errors

---

## Sections

| Section | Description |
|---|---|
| Hero | Headline, sub-copy, and call-to-action button |
| About | Practice philosophy and office atmosphere photo |
| Services | Service cards — Individual, Couples, Anxiety & Depression, Trauma, Grief |
| Bio | Therapist headshot, credentials, and biography |
| FAQ | Expandable frequently asked questions |
| Contact | Contact details, inline Calendly booking widget, and general inquiry form |

---

## Project Structure

```
/
└── index.html      # Entire site — HTML, CSS, and JS in one file
└── README.md       # This file
```

---

## Contact Form Setup

The form submits to [Formspree](https://formspree.io). To connect it to your account:

1. Sign in at [formspree.io](https://formspree.io) and create a new form.
2. Copy your form endpoint ID (e.g. `xrpgzjww`).
3. In `index.html`, replace the endpoint in these two places if you change accounts:
   - `<form … action="https://formspree.io/f/<YOUR_ID>">`
   - `fetch('https://formspree.io/f/<YOUR_ID>', …)`
4. In the Formspree dashboard, go to **Settings → Allowed Origins** and add your GitHub Pages URL to restrict submissions to your domain.
5. Configure email notifications under **Settings → Notifications**.

> **Privacy notice:** Formspree's standard plan is not HIPAA-secure. The form already displays a notice instructing users not to submit sensitive health information.

---

## Calendly Booking Widget

The `#contact` section includes a Calendly inline widget that lets visitors self-book sessions without leaving the page. The widget sits above the general inquiry form in the right column.

### How it works

The widget is powered by three HTML elements added to `index.html`:

1. A `<link>` tag in `<head>` loading the Calendly widget CSS.
2. A `<script>` tag near `</body>` loading the Calendly widget JS.
3. A `<div class="calendly-inline-widget" data-url="...">` inside `#contact` that Calendly targets to render the booking calendar.

### Updating the Calendly URL

The `data-url` attribute controls which Calendly page is displayed. To find or update it, search `index.html` for `calendly-inline-widget`:

```html
<div
  class="calendly-inline-widget"
  data-url="https://calendly.com/bowserryan12"
  style="min-width:320px;height:630px;"
></div>
```

The current value — `https://calendly.com/bowserryan12` — is the **profile page URL**, which shows all event types at once and lets the visitor choose.

### Event types

The following event types are currently configured in the Calendly account and will appear automatically in the widget:

| Event | URL |
|---|---|
| New Client Intake | `https://calendly.com/bowserryan12/new-client-intake` |
| 30 Min | `https://calendly.com/bowserryan12/30min` |
| Family Therapy Session | `https://calendly.com/bowserryan12/family-therapy-session` |

> **No code changes are needed when event types are added, renamed, reordered, or removed in Calendly.** The widget always reflects the current state of the account automatically.

### Paid tier considerations

Calendly's free tier is functional but includes Calendly branding in the widget. If you need:
- **Custom branding / remove Calendly logo** → Standard plan (~$10/mo)
- **HIPAA compliance + BAA** → consider migrating to SimplePractice (see below)

### Migrating to SimplePractice

The integration is structured to make a future migration straightforward. When you're ready:

**Remove** these two lines from `index.html`:
- The `<link>` tag for Calendly CSS (in `<head>`, marked with a comment)
- The `<script>` tag for Calendly JS (near `</body>`, marked with a comment)

**Replace** the Calendly `<div>` with your SimplePractice embed markup.

**Keep as-is** — no changes needed to:
- `id="book-session"` on the wrapper `<div>`
- `href="#book-session"` on the nav and hero "Schedule an Appointment" buttons
- The `.calendly-inline-widget` responsive CSS in the `@media` block (remove or repurpose as needed)

---

## Deploying to GitHub Pages

1. Push this repository to GitHub.
2. Go to **Settings → Pages**.
3. Under **Source**, select `Deploy from a branch`, choose `main`, and set the folder to `/ (root)`.
4. Click **Save**. GitHub will publish the site within a minute or two.
5. Your site will be available at `https://rbowser12.github.io/playground/`.

---

## Customisation Checklist

Before going live, replace all placeholder content in `index.html`:

- [ ] Practice name (nav logo, footer, page title)
- [ ] Therapist name, credentials, and biography
- [ ] Hero headline and sub-copy
- [ ] Services listed (add, remove, or rename cards)
- [ ] FAQ questions and answers
- [ ] Phone number, email address, office hours, and location
- [ ] Insurance providers accepted
- [ ] State name for telehealth eligibility
- [ ] Office / atmosphere photo (`about-img`)
- [ ] Therapist headshot (`bio-photo`)
- [ ] Footer address and phone number
- [ ] Formspree form endpoint ID
- [ ] Calendly profile URL in the booking widget `data-url` attribute

---

## Accessibility

This site is built with accessibility in mind:

- `lang="en"` on `<html>`
- Correct heading hierarchy (`h1` → `h2` → `h3`)
- All form inputs have associated `<label>` elements
- Skip-to-content link for keyboard users
- ARIA landmark labels on `<nav>`
- Decorative SVGs marked `aria-hidden="true"`
- Duplicate call-to-action links have unique `aria-label` values
- Form error messages use `role="alert"` for screen reader announcement

---

## License

This project is released for personal and commercial use. No attribution required.
