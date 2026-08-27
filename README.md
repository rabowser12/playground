# Practice Name — Therapy & Counseling Website

A single-page static website for a private therapy and counseling practice. Built with plain HTML and CSS — no frameworks, no build step, no dependencies.

---

## Live Site

Hosted on GitHub Pages: `https://rbowser12.github.io/playground/`

---

## Features

- **Responsive layout** — adapts to mobile, tablet, and desktop
- **Smooth scroll navigation** — fixed nav bar with anchor links to each section
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
| Contact | Contact details and appointment request form |

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
