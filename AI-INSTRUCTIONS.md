# Instructions for an AI building a site on nano-cms

You are a **site builder**. Your job is to produce one file: `index.html`.

Before you write a single line of code, read these two files in full:

1. **The content:** https://raw.githubusercontent.com/etxgmg/nano-cms/main/content.json
2. **The contract:** https://raw.githubusercontent.com/etxgmg/nano-cms/main/SITE-BUILDER-GUIDE.md

The contract explains every rule you must follow. The content file shows you exactly which blocks and fields exist and what the site owner can edit.

---

## Your task

Build a complete, production-ready `index.html` that:

- Fetches `./content.json` on page load and renders the entire page from it
- Applies the visual design described in the site brief below
- Contains all CSS and JavaScript inline in the single file — no external build step, no frameworks, no bundlers
- Works on GitHub Pages without modification

---

## Hard rules — non-negotiable

- **No visible text may be hardcoded in HTML.** Every string the site visitor reads must come from `content.json`.
- **No visible image may be hardcoded.** Every image path must come from `content.json`.
- **Field names in `editable` must not be changed or added to.** The admin interface depends on them.
- **`block.id` values must not be renamed.** They are stable identifiers used by the admin.
- **Empty fields must not crash the site.** Render defensively with `?? ''` or conditional checks.
- **Do not modify anything in `admin/`.** That folder belongs to the CMS, not the site.

---

## Site brief

*The person commissioning this site describes the desired look and feel here. Replace this section with the actual brief before handing these instructions to an AI.*

**Example:**
> Clean, minimal design for a Swedish carpentry business. Warm off-white background, dark wood-brown headings, serif font for headings and sans-serif for body text. Mobile-first. The hero section should feel generous and calm. Contact section at the bottom with a soft dark background.

---

## Checklist before you deliver

- [ ] All visible text is fetched from `content.json`
- [ ] All visible images are fetched from `content.json`
- [ ] No field names inside `editable` have been changed
- [ ] Empty fields do not crash the site
- [ ] `block.id` values match exactly between `content.json` and `index.html`
- [ ] `fetch('./content.json')` works both locally and on GitHub Pages
- [ ] The file `admin/index.html` has not been touched
- [ ] The output is a single self-contained `index.html`
