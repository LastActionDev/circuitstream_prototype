# Spec Delta

## Purpose

Keeps an up-to-date record of the current price of one item sold on one hardware store's website, so the owner can see it without visiting the store.

## ADDED Requirements

### Requirement: Single configured item
The system SHALL track exactly one item, identified by the URL of its product page on one hardware store website. The URL MUST be set through configuration so the item or store can change without code changes.

#### Scenario: Item is configured
- **WHEN** the configuration contains a product page URL
- **THEN** the system tracks the price of the item at that URL

#### Scenario: Item is changed
- **WHEN** the owner replaces the configured URL with a different product page URL
- **THEN** the next price check reads the price of the new item and the old item is no longer tracked

#### Scenario: No item configured
- **WHEN** no product page URL is configured
- **THEN** no price check runs and the system reports that no item is configured

### Requirement: Scheduled price check
The system SHALL check the item's current price automatically at least once a day without any action from the owner.

#### Scenario: Daily check
- **WHEN** a day passes since the last price check
- **THEN** the system reads the item's current price from its product page and records it with the time it was checked

### Requirement: Price is read from the product page
The system SHALL read the item's current selling price, in the store's currency, from the configured product page. When the page shows a sale price alongside a regular price, the price recorded MUST be the price a buyer would pay.

#### Scenario: Regular price
- **WHEN** the product page shows a single price
- **THEN** the system records that price

#### Scenario: Item on sale
- **WHEN** the product page shows both a regular price and a lower sale price
- **THEN** the system records the sale price

### Requirement: Failed checks keep the last known price
The system SHALL keep the most recent successfully read price when a price check fails, and MUST NOT replace it with an empty, zero or guessed value.

#### Scenario: Product page unavailable
- **WHEN** the product page cannot be reached or returns an error during a check
- **THEN** the last known price and its check time are kept unchanged

#### Scenario: Price not found on the page
- **WHEN** the product page loads but no price can be found on it
- **THEN** the last known price and its check time are kept unchanged and the failure is recorded so it can be investigated

### Requirement: Polite access to the store
The system SHALL request the product page no more than a few times per day and MUST NOT check pages other than the configured product page.

#### Scenario: Normal operation
- **WHEN** the system runs for a full day
- **THEN** it has requested only the configured product page, and no more than a few times
