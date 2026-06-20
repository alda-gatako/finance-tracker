# Student Ledger — Finance Tracker

A no-framework, offline-first budget tracker built with vanilla HTML, CSS, and JavaScript (ES modules). Built for the regex / HTML-CSS / JavaScript fundamentals assignment.

**Live demo:**  https://alda-gatako.github.io/finance-tracker
**Repo https:**//github.com/alda-gatako/finance-tracker

---

## Chosen theme

**1) Student Finance Tracker** — tracks expenses with description, amount, category, and date. Includes currency conversion (base + 2 manual rates) and a monthly spending cap with live status updates.

---

## Features

- **About** page with purpose and contact info.
- **Dashboard**: total records, total spent, top category, last-7-days bar chart, spending-cap "ruler" with ARIA live status (polite when under cap, assertive when over), and converted totals in two extra currencies.
- **Records**: sortable table (desktop) / cards (mobile, <600px) with live regex search and `<mark>`-highlighted matches.
- **Add/Edit form**: 4 validated fields with inline error messages, keyboard-friendly, edits an existing record or adds a new one.
- **Settings**: base currency, manual exchange rates, monthly cap, JSON import/export with structural validation.
- **Persistence**: every change auto-saves to `localStorage`. Refreshing the page does not lose data.
- **Accessibility**: skip link, semantic landmarks, visible focus rings, ARIA live regions, full keyboard operability, ≥3 responsive breakpoints (360px / 768px / 1024px), respects `prefers-reduced-motion`.

---

## Regex catalog

| # | Purpose | Pattern | Example match |
|---|---------|---------|----------------|
| 1 | Description: no leading/trailing spaces, no doubled spaces | `^\S(?:.*\S)?$` | ✅ `"Lunch at cafeteria"` · ❌ `" Lunch "` |
| 2 | Amount: positive number, ≤2 decimals, no leading zeros | `^(0\|[1-9]\d*)(\.\d{1,2})?$` | ✅ `12.50` · ❌ `01.5`, `12.505` |
| 3 | Date: strict `YYYY-MM-DD` with valid month/day range | `^\d{4}-(0[1-9]\|1[0-2])-(0[1-9]\|[12]\d\|3[01])$` | ✅ `2026-06-20` · ❌ `2026-13-01` |
| 4 | Category: letters with single spaces/hyphens between words | `^[A-Za-z]+(?:[ -][A-Za-z]+)*$` | ✅ `Self-Care` · ❌ `Food2` |
| 5 (advanced — back-reference) | Catch an accidentally duplicated word in the description | `\b(\w+)\s+\1\b` (case-insensitive) | matches `"the the bus"` |
| — (search example) | Find amounts with cents | `\.\d{2}\b` | matches descriptions/categories containing two-digit decimals if typed as search text |
| — (search example) | Find beverage-related entries | `(coffee\|tea)` (case-insensitive) | matches `"Coffee with study group"`, `"Tea and a sandwich"` |

All live search input is compiled with `try/catch` (`compileSafeRegex` in `scripts/validators.js`); an invalid pattern shows an inline error instead of crashing the page, and the table keeps showing the last valid result set.

---

## Keyboard map

| Key | Action |
|---|---|
| `Tab` / `Shift+Tab` | Move focus through skip link, nav tabs, form fields, table actions |
| `Enter` / `Space` | Activate the focused button/tab |
| `←` / `→` (while a nav tab is focused) | Move between tabs (roving tabindex) |
| `Tab` into search box, then type | Live-filters the table/cards as you type |

The very first link on the page is **"Skip to content,"** which jumps past the header/nav straight to `<main>`.

---

## Accessibility notes

- Landmarks: `<header>`, `<nav>`, `<main>`, `<section>` (per panel), `<footer>`.
- All inputs have associated `<label>` elements; errors are linked via `aria-describedby` and announced with `role="alert"`.
- Invalid fields get `aria-invalid="true"` and a red-tinted border (not color alone — also a text error message).
- Dashboard cap status uses `aria-live="polite"` normally and switches to `aria-live="assertive"` (with re-render) when spending exceeds the cap.
- Tabs follow the WAI-ARIA Tabs pattern (`role="tablist"/"tab"/"tabpanel"`, `aria-selected`, roving `tabindex`).
- Color contrast: ink (`#1B1B1B`) on paper (`#FAF7F0`) and all status colors were checked against WCAG AA for normal text.
- `prefers-reduced-motion: reduce` disables/shortens all transitions and animations.
- Mobile-first CSS with breakpoints at ~360px (base), 768px (tablet — 4-col stats), 1024px (desktop refinements). Table switches to stacked cards below 600px.

---

## Data model

```json
{
  "id": "rec_0001",
  "description": "Lunch at cafeteria",
  "amount": 12.50,
  "category": "Food",
  "date": "2026-06-20",
  "createdAt": "2026-06-20T12:05:00.000Z",
  "updatedAt": "2026-06-20T12:05:00.000Z"
}
```

## File structure

```
index.html   – the whole app: semantic HTML + all CSS (in <style>) + all JS modules (in <script type="module">)
tests.html   – assertion-based tests, with validators.js/search.js logic inlined the same way
seed.json    – 12 example records incl. edge cases (leap day, 0.99, hyphenated category, duplicate-word typo)
README.md    – this file
```

All CSS and JavaScript are intentionally inlined directly into `index.html` and `tests.html` (rather than split into separate `styles/`/`scripts/` files) so the project is a flat, dependency-free set of files — simpler to host and to grade. Internally the JavaScript is still organized into clearly commented sections mirroring a modular design: storage (localStorage), validators (regex rules), search (regex filter + highlight), state (single source of truth + pub/sub), and ui (rendering/events).

## How to run

1. Clone the repo and open `index.html` directly in a browser — no build step, no server required, since everything is self-contained in one file.
2. To load the seed data: go to **Settings → Import JSON** and select `seed.json`.
3. To run tests: open `tests.html` directly in a browser. Results print on-page and to the console.

## Deployment

Deployed via **GitHub Pages** (Settings → Pages → deploy from `main` branch, root folder).

## Individual work statement

This repository has a single contributor (the author's GitHub account). No collaborators were added.
