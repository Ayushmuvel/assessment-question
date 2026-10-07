# FS-01: P2P Wallet Transfer

**Role:** Full Stack Engineer (MERN)  
**Stack:** MongoDB, Express.js, React, Node.js. TypeScript is preferred.

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **FS-01** in your repository README and in your reply email.

## Context
Users of a digital wallet send money to each other. The network is unreliable, so the mobile and web clients retry requests. A retry must never move money twice.

## Requirements

**Backend (Node.js + Express + MongoDB)**

- `POST /auth/register` and `POST /auth/login`: return a JWT. Two roles: `user` and `admin`.
- `GET /wallet`: the logged-in user's balance. New users start with a test balance of ₹10,000.
- `POST /wallet/transfer`
  - Body: `{ "toUserId": "...", "amount": 500, "note": "Lunch" }`
  - Header: `Idempotency-Key: <uuid>`
  - Rules: amount between ₹1 and ₹50,000; no transfers to yourself; the sender must have enough balance.
  - Sending the same `Idempotency-Key` again returns the **original result** and does not move money again.
- `GET /transactions?page=1&limit=20&status=SUCCESS`: the user's own transactions, newest first.
- `GET /admin/transactions`: all transactions (admin only).

**Frontend (React)**

- Login and register pages.
- Dashboard with balance card and recent transactions.
- Send Money form with validation. The submit button is disabled while a request is in flight.
- Clear loading, empty, error and success states.

## Bonus
- Use a MongoDB transaction for debit + credit.
- Unit or integration tests (Jest + Supertest) for the transfer rules and the idempotency behaviour.
- Docker Compose to run everything with one command.

## Questions you should be ready to answer
- What happens if the server crashes after debiting the sender but before crediting the receiver?
- What happens if two transfers from the same wallet run at the same moment?
