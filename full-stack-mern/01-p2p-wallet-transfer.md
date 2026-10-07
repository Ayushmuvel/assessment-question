# FS-01: P2P Wallet Transfer

**Role:** Full Stack Engineer (MERN) · **Time:** about 3 hours (additional tasks are optional)

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **FS-01** in your repository README and in your reply email.

## Scenario

Users send money to each other from a wallet. The network is unreliable, so clients retry requests. A retry must never move money twice.

## Must have

**Backend (Node.js + Express + MongoDB)**

- Seed 3 users with a balance of ₹10,000 each. A simple login that returns a JWT is enough (no registration).
- `GET /wallet`: the logged-in user's balance.
- `POST /wallet/transfer` with body `{ "toUserId": "...", "amount": 500 }` and header `Idempotency-Key: <uuid>`
  - amount between ₹1 and ₹50,000, no transfer to yourself, enough balance required
  - the same `Idempotency-Key` sent again returns the **original result** and does not move money again
- `GET /transactions`: the user's transactions, newest first.

**Frontend (React)**

- One page: balance, a Send Money form, and the transaction list.
- The submit button is disabled while the request is in flight. Show success and error messages.

## Additional tasks (optional, plus points)

Not required. Each one you complete counts in your favour.

- Add MongoDB indexes for your main queries and explain each one in your README.
- Debit and credit inside a MongoDB transaction.
- Tests for the transfer rules and the idempotency behaviour.
- TypeScript.

## Answer in your README (a few lines each)

1. What happens if the server crashes after debiting the sender but before crediting the receiver?
2. What happens if two transfers from the same wallet run at the same moment?

If you run out of time, list what you skipped and how you would build it.
