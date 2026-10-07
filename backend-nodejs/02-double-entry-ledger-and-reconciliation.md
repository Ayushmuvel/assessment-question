# BE-02: Double-Entry Ledger

**Role:** Backend Engineer (Node.js) · **Time:** about 3 hours (bonus is optional)

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **BE-02** in your repository README and in your reply email.

## Scenario

Every money movement in a wallet system is recorded in a ledger. Balances are calculated from ledger entries, so the ledger must always be correct.

## Must have

- `POST /accounts`: create an account `{ name }`.
- `POST /accounts/:id/deposit`: add money (simulates a top-up from a bank).
- `POST /transfers` with body `{ fromAccountId, toAccountId, amount, reference }`
  - writes **two entries**: a debit and a credit of the same amount, in one atomic database transaction
  - an account can never go below zero
  - the same `reference` is never processed twice
- `GET /accounts/:id/balance`: calculated from ledger entries.
- `GET /accounts/:id/statement`: entries with a running balance.
- Store amounts safely (integer paise or a decimal type, never floating point).

## Bonus (optional)

- `POST /reconciliation`: upload a small CSV (`reference,amount`) and return matched, missing and mismatched entries.
- A test showing that many parallel transfers from one account never overdraw it.

## Answer in your README (a few lines each)

1. Why calculate the balance from entries instead of storing one number? What would you do when it gets slow?
2. How did you prevent two parallel transfers from overdrawing an account?

If you run out of time, list what you skipped and how you would build it.
