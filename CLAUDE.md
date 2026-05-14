# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-file CV generator web app for SevenPro. Everything lives in `index.html` — no build step, no package manager, no framework. Open `index.html` directly in a browser or serve it locally (e.g. `python3 -m http.server`) to run the app.

## Key files

- `index.html` — the entire application (HTML + CSS + JS, ~800 lines)
- `template.docx` — Word template with docxtemplater placeholders; required for DOCX export
- `Seven-Pro-Logotypes.svg` — company logo referenced inline

## Architecture

The app is split into a left form panel and a right live-preview panel.

**State** — a plain JS object `state` (defined near line 186) holds all CV data: `name`, `position`, `recruitmentFeedback`, `technicalFeedback`, `technicalSkills[]`, `experiences[]`, `education[]`, `certifications[]`, `languages[]`, and a `show{}` map to toggle section visibility in DOCX output.

**Experience structure** — `state.experiences` is a two-level hierarchy: each entry has a `company` string and a `projects[]` array. `collectData()` flattens this into a single experiences list for DOCX templating.

**Preview** — `buildPreview()` renders the `state` into a `.cv` HTML block inside `#cvOut`. It is debounced via `schedulePreview()` (300 ms) and called on every form input.

**DOCX export** — `genDocx()` fetches `template.docx`, passes `collectData()` output to docxtemplater (with `paragraphLoop:true` and `linebreaks:true`), and triggers a browser download.

**PDF export** — `genPdf()` clones the `.cv` preview element, feeds it to html2pdf.js (html2canvas + jsPDF), then adds a per-page branded footer using raw jsPDF calls.

## CDN dependencies (no local install needed)

- [PizZip 3.1.4](https://unpkg.com/pizzip@3.1.4/dist/pizzip.js)
- [docxtemplater 3.45.1](https://unpkg.com/docxtemplater@3.45.1/build/docxtemplater.js)
- [html2pdf.js 0.10.1](https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js)
- Google Fonts — Montserrat

## CV preview CSS conventions

Preview styles follow the DOCX template exactly. Key class patterns:
- `.cv-sec-title` — section headings (14pt bold purple `#6b3fa0`)
- `.cv-sublabel` — project/role labels (12pt semibold)
- `.cv-text` — body copy (11pt gray `#666666`, `white-space:pre-wrap`)
- `.cv-tech` — gray technology box with border (must set `backgroundColor` explicitly in html2canvas `onclone` to render correctly)
- `.cv-co-block` / `.cv-project` / `.cv-ul` — marked with `pagebreak.avoid` in PDF export to prevent mid-element page breaks
