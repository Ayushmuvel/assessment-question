# FE-01: Merchant Transactions Dashboard

**Role:** Frontend Engineer (React / Next.js) · **Time:** about 3 hours (additional tasks are optional)

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **FE-01** in your repository README and in your reply email.

## Scenario

Merchants need to find specific transactions quickly among hundreds of them.

## Must have

- Next.js + TypeScript. Tailwind CSS or any styling approach.
- Mock data: about 200 transactions (id, date, customer, amount, method `UPI | CARD | WALLET`, status `SUCCESS | FAILED | PENDING`), served from a Next.js route handler or a JSON file.
- Summary cards: total volume, success rate, failed count.
- Transactions table with:
  - search by transaction ID or customer
  - filter by status
  - pagination
- Filters and page number are kept in the URL, so a refresh keeps the view.
- Loading, empty and error states.
- Works on mobile width.

## Additional tasks (optional, plus points)

Not required. Each one you complete counts in your favour.

- Mock login with protected routes, and one role difference (for example, only an `admin` sees an "Export CSV" button).
- Date range filter.
- A daily volume chart.
- One test (React Testing Library or Playwright).

## Answer in your README (a few lines each)

1. Which parts are Server Components and which are Client Components, and why?
2. What would you change if there were 1 million transactions instead of 200?

If you run out of time, list what you skipped and how you would build it.
