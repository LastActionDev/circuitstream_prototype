# Proposal

## Why

I want to keep an eye on the price of one specific item sold on a hardware store's website without revisiting the product page myself. A tiny personal site that always shows the item's latest price, with a link back to the product, answers "what does it cost right now?" at a glance.

## What Changes

- Add a single configured item to track: one product page URL on one hardware store website (the store is still to be decided, so the item is set through configuration rather than hard-coded).
- Add an automatic price check that reads the item's current price from its product page on a regular schedule and keeps the most recent result.
- Add a one-page website, for the owner only, that shows the item's price in large lettering with a link to the original product page underneath, and nothing else.
- Out of scope: tracking more than one item or more than one store, price history or charts, alerts or notifications, user accounts or sign-in, and any public or shared use.

## Capabilities

### New Capabilities
- `price-tracking`: Which item is tracked, how its current price is read from the store's product page, how often that happens, and what is kept when a check fails.
- `price-display`: The single page that presents the tracked item's current price in large lettering with a link to the original product page.

### Modified Capabilities
<!-- None: the project has no existing specs. -->

## Impact

- New project: the repository currently contains only the OpenSpec setup, so this change introduces the first application code, its configuration and its deployment.
- External dependency on the chosen hardware store's product page. How the price is read depends on that site's page structure, whether it allows automated access, and its terms of use, which can only be confirmed once the store is chosen.
- Needs somewhere to run the scheduled check and serve the page (for example a free hosting plan with scheduled jobs).
