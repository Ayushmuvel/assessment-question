# FL-03: Offline-First Transaction History

**Role:** Flutter Mobile App Developer · **Time:** about 3 hours (bonus is optional)

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **FL-03** in your repository README and in your reply email.

## Scenario

Users check their transaction history often, sometimes with no internet. The app must open fast and scroll smoothly through many transactions.

## Must have

- A mock API (inside the app) returning **about 500 transactions**, paginated (`page`, `limit`), with a short delay. Fields: id, date, merchant, amount, type (debit / credit), status.
- **Transactions list:**
  - infinite scroll with pagination
  - search by merchant
  - filter by type (debit / credit)
- **Offline-first:** cache data locally (Hive, sqflite, Isar or similar). The app opens with cached data, refreshes in the background, and shows "Last updated at …".
- **Detail screen** for a transaction.
- Submit a 2–3 minute screen recording (an APK is welcome).

## Bonus (optional)

- Group the list by day with daily totals.
- Monthly spend chart.
- App lock with biometrics or PIN.
- Unit tests for the caching logic.

## Answer in your README (a few lines each)

1. How do you decide when cached data is too old to show?
2. How did you keep scrolling smooth? How would you check for performance problems?

If you run out of time, list what you skipped and how you would build it.
