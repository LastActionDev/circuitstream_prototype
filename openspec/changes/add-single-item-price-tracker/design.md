# Design

## Context

The repository is empty apart from the OpenSpec setup and is hosted on GitHub (public). There is no server, database or budget. The hardware store is not chosen yet, so nothing can depend on one store's page layout. Requirements are in `specs/price-tracking/spec.md` and `specs/price-display/spec.md`.

## Goals / Non-Goals

**Goals:**
- Run at no cost with nothing to maintain day to day.
- Keep the store-specific part (reading the price off the page) small and isolated, so choosing or changing the store touches one place.

**Non-Goals:**
- A backend, database or API of our own.
- Supporting several stores at once or a plugin system for stores.

## Decisions

### 1. Static page + scheduled job instead of a server
A scheduled GitHub Actions workflow runs a small Node.js script once a day. It fetches the product page, reads the price and writes it to `data/price.json` in the repository. The website is a single static `index.html` served by GitHub Pages that reads `data/price.json`.

- **Why:** free, no server to keep running, and the repo we already have does both jobs. The daily commit doubles as a simple record of every check.
- **Alternatives:** a hosted app (Vercel, Render) with its own scheduler and storage, more moving parts for one number; fetching the price live in the browser when the page opens, blocked by the store's cross-site rules (CORS) and would hit the store on every visit.

### 2. One configuration file
`config.json` holds the product URL and, if needed, a CSS selector for the price. Changing the item or store means editing this file only.

### 3. Price extraction: structured data first, selector second
Most large retailers embed product data for search engines (JSON-LD `Product` → `offers.price`, or `itemprop="price"`). The script reads that first, because it is stable across redesigns and already holds the price a buyer pays. If the store has none, it falls back to the CSS selector from `config.json`.

- **Alternative:** scraping visible text only, which breaks whenever the page design changes.

### 4. Plain HTTP fetch, headless browser only if needed
Start with a plain HTTP request. If the chosen store only renders the price with JavaScript, or rejects plain requests, switch that step to a headless browser (Playwright) inside the same workflow. This is decided once the store is chosen (see tasks, section 1).

### 5. Stored result
`data/price.json` holds `url`, `price`, `currency`, `checkedAt`, plus `lastError` and `lastErrorAt` for failed checks. On failure the script updates only the error fields, so the last good price stays (per spec).

### 6. Page
One `index.html` with inline CSS and a few lines of script. The price is sized with `clamp()` so it fills the screen on a phone and a desktop, the link sits underneath, and a `noindex` robots meta tag keeps it out of search results.

## Risks / Trade-offs

- [The store blocks automated requests, especially from GitHub's servers] → Check one request from a workflow run before building further (task 1.3); if blocked, try the headless browser; if still blocked, pick another store or run the check from the owner's computer.
- [The store's terms of use forbid automated access] → Review them when choosing the store (task 1.2) and pick another store if so.
- [Page layout changes break extraction] → Structured-data-first extraction; failures keep the last price and record the error, which shows up in the workflow's run history.
- [GitHub Pages on a public repo means the page URL and the price data are publicly reachable, only unlisted] → Acceptable for a non-sensitive price; `noindex` keeps it out of search. If real privacy is needed later, move hosting behind a login.
- [GitHub pauses scheduled workflows after 60 days with no repository activity] → The daily data commit counts as activity; the workflow can also be run by hand.

## Migration Plan

New project, nothing to migrate. Deploy by enabling GitHub Pages on the repo and the scheduled workflow. Roll back by disabling the workflow or Pages.

## Open Questions

- Which hardware store and which item? Needed before task 1.3 onward, but it does not change the specs or this approach.
- Time of day for the check (default: early morning Toronto time).
