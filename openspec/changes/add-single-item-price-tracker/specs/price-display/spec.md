# Spec Delta

## Purpose

Shows the owner the tracked item's current price at a glance on a single page, with a way to jump to the item on the store's website.

## ADDED Requirements

### Requirement: Price shown in large lettering
The website SHALL consist of a single page whose main content is the tracked item's latest recorded price, shown in large lettering as the most prominent element, formatted in the store's currency.

#### Scenario: Price available
- **WHEN** the owner opens the page and a price has been recorded
- **THEN** the latest recorded price is shown in large lettering, formatted as currency (for example "$249.99")

#### Scenario: Readable on a phone
- **WHEN** the owner opens the page on a phone-sized screen
- **THEN** the price is fully visible in large lettering without zooming or scrolling sideways

### Requirement: Link to the original item
The page SHALL show a link underneath the price that opens the item's product page on the store's website.

#### Scenario: Owner follows the link
- **WHEN** the owner selects the link under the price
- **THEN** the item's original product page opens

### Requirement: Nothing else on the page
The page SHALL NOT show content other than the price and the link, such as charts, history, item photos, ads or navigation.

#### Scenario: Page contents
- **WHEN** the owner opens the page
- **THEN** the only visible content is the price and the link underneath it

### Requirement: No price recorded yet
The page SHALL show a clear placeholder instead of a price when no price has been recorded yet, and still show the link.

#### Scenario: First visit before any check
- **WHEN** the owner opens the page before the first successful price check
- **THEN** a placeholder such as "—" appears where the price goes and the link is still shown

### Requirement: Private by default
The page SHALL be for the owner only: it MUST NOT be linked from anywhere public and MUST ask search engines not to index it.

#### Scenario: Search engine visits the page
- **WHEN** a search engine crawler loads the page
- **THEN** the page tells it not to index the page
