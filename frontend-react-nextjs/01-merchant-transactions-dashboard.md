# FE-01: Merchant Transactions Dashboard

**Role:** Frontend Engineer (React / Next.js)  
**Stack:** Next.js (App Router preferred), React, TypeScript, Tailwind CSS (or a comparable styling approach).

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **FE-01** in your repository README and in your reply email.

## Context
Merchants log in to see how their business is doing and to find specific transactions quickly. Some merchants have thousands of transactions.

## Requirements

- Login page with validation. Protect all dashboard routes; redirect unauthenticated users to login.
- **Overview page:** summary cards (total volume, number of transactions, success rate, failed count) for a selected date range.
- A chart of daily volume for the last 30 days (any chart library).
- **Transactions page:**
  - Table: transaction ID, date, customer, amount, method (UPI / Card / Wallet), status.
  - Search by transaction ID or customer, filter by status, method and date range.
  - Server-side style pagination (at least 500 mock rows).
  - Filters and page are stored in the URL, so a link can be shared and reloads keep the view.
- Transaction detail drawer or page.
- Loading (skeletons), empty, error (with retry) and success states.
- Responsive down to 360 px width.

## Bonus
- Export the filtered list as CSV.
- TanStack Query (or similar) for data fetching and caching.
- Tests with React Testing Library or Playwright.
