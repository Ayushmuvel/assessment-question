# BE-02: Double-Entry Ledger & Reconciliation

**Role:** Backend Engineer (Node.js)  
**Stack:** Node.js with Express or NestJS. MongoDB, PostgreSQL or MySQL. TypeScript is preferred.

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **BE-02** in your repository README and in your reply email.

## Context
Every money movement in a wallet system is recorded in a ledger. Balances are derived from ledger entries. At the end of the day, we reconcile our ledger against the bank's settlement file.

## Requirements

- Accounts: `POST /accounts` (types: `USER_WALLET`, `MERCHANT`, `FEES`, `BANK_SETTLEMENT`).
- `POST /transfers`: `{ fromAccountId, toAccountId, amount, reference }`
  - Write **double-entry** records: one debit and one credit of the same amount, in a single atomic transaction.
  - A user wallet can never go below zero.
  - Optional fee: if `fee` is provided, also credit the `FEES` account.
- `GET /accounts/:id/balance`: computed from ledger entries (you may cache it, but it must be correct).
- `GET /accounts/:id/statement?from=&to=`: entries with a running balance.
- `POST /reconciliation`: upload a CSV bank settlement file (`reference,amount,date`). Return:
  - matched entries
  - entries in our ledger but missing in the bank file
  - entries in the bank file but missing in our ledger
  - amount mismatches

Include a sample CSV in your repository.

## Bonus
- Prove with a test that 100 concurrent transfers from the same wallet never overdraw it.
- Store amounts safely (integer paise or a decimal type, never floating point). Explain your choice.
