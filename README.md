# Price tracker

A one-page site that shows the current price of one item from a hardware store, with a link to the item underneath. For personal use only.

Status: placeholder page. The store, item and daily price check are not set up yet (see `openspec/changes/add-single-item-price-tracker/`).

## Files

- `index.html` — the page. Shows the price from `data/price.json`, or "—" when there is none.
- `data/price.json` — the latest price (`price`, `currency`, `url`, `checkedAt`). Filled in by the price check later.
- `config.json` — the product page URL to track (`productUrl`) and an optional CSS selector for the price (`priceSelector`). Empty until the store and item are chosen.

## Run it locally

The page loads `data/price.json`, so open it through a small local web server rather than double-clicking `index.html`. From this folder, run either:

```sh
python3 -m http.server 8000
```

or, with Node.js:

```sh
npx serve -l 8000
```

Then open http://localhost:8000 in your browser.
