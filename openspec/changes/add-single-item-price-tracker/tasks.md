# Tasks

## 1. Choose the store and item

- [ ] 1.1 Owner picks the hardware store and the item; verify by recording the product URL in `config.json`
- [ ] 1.2 Review the store's terms of use and `robots.txt` for automated access to product pages; verify the outcome (allowed / not allowed) is noted in `README.md`, and pick another store if not allowed
- [ ] 1.3 From a test GitHub Actions run, fetch the product page with a plain HTTP request and check whether the price is in the HTML (structured data or visible); verify the run log shows the price, or record that a headless browser is needed

## 2. Price check script

- [ ] 2.1 Set up a minimal Node.js project (`package.json`, `scripts/check-price.js`, `config.json`, `data/`); verify `npm install` and `node scripts/check-price.js --help` succeed
- [ ] 2.2 Read the price from structured data (JSON-LD `offers.price`, then `itemprop="price"`), falling back to the CSS selector in `config.json`; verify with unit tests against saved sample pages covering a regular price, a sale price and a page with no price
- [ ] 2.3 Use a headless browser (Playwright) for the fetch only if task 1.3 found it necessary; verify the script prints the live price when run locally
- [ ] 2.4 Write `data/price.json` on success, and on failure update only `lastError`/`lastErrorAt` while keeping the last good price; verify with tests for an unreachable page and a page with no price
- [ ] 2.5 Exit with a clear message and write nothing when no URL is configured; verify with a test using an empty config
- [ ] 2.6 Document the config fields and how to run the check by hand in `README.md`; verify the documented command works as written

## 3. Scheduled workflow

- [ ] 3.1 Add `.github/workflows/check-price.yml` running the script once a day (early morning Toronto time) and on manual trigger, committing `data/price.json` when it changes; verify a manual run commits an updated file
- [ ] 3.2 Confirm the workflow requests only the configured product page, once per run; verify from the run log

## 4. Price page

- [ ] 4.1 Build `index.html`: price in large lettering read from `data/price.json`, formatted as currency, with the product link underneath and nothing else; verify in a browser at desktop and phone widths
- [ ] 4.2 Show "—" in place of the price when no price is recorded yet, keeping the link; verify by opening the page with an empty `data/price.json`
- [ ] 4.3 Add a `noindex` robots meta tag; verify it is present in the served page

## 5. Deploy and check end to end

- [ ] 5.1 Enable GitHub Pages for the repo and note the page URL in `README.md`; verify the page loads at that URL
- [ ] 5.2 Run the workflow by hand and reload the page; verify it shows the item's current price matching the store's product page and the link opens that product page

## Workflow follow-up

- Archive the change once the owner has reviewed the live page.
