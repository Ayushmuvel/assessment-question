# FL-03: Offline-First Transaction History & Insights

**Role:** Flutter Mobile App Developer  
**Stack:** Flutter and Dart, with Bloc, Riverpod, Provider or a similar state management approach.

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **FL-03** in your repository README and in your reply email.

## Context
Users want to understand their spending. Our app must work smoothly with thousands of transactions, even offline.

## Requirements

- Mock API returning **at least 2,000 transactions**, paginated (`page`, `limit`), with fields: id, date, merchant, category, amount, type (debit / credit), status.
- **Transactions list:**
  - infinite scroll with pagination
  - search by merchant, filter by category, type, status and date range
  - grouped by day with daily totals
  - smooth scrolling with no jank (use DevTools to check)
- **Offline-first:** data is cached locally; the app opens instantly with cached data and refreshes in the background. Show "Last updated at ..." text.
- **Insights screen:** monthly spend by category (pie or bar chart) and a month-over-month comparison.
- **Transaction detail** with a "Report a problem" form that queues the report while offline and sends it when the network returns.
- **App lock:** biometrics or PIN when the app opens.
- Responsive layout for phones and tablets.

## Bonus
- Unit tests for the caching and sync logic.
- Platform channel (or plugin) example to read the device's battery or network type and show it in a debug screen.
