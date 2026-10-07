# BE-03: Transaction Risk & Velocity Checks

**Role:** Backend Engineer (Node.js)  
**Stack:** Node.js with Express or NestJS. MongoDB, PostgreSQL or MySQL. TypeScript is preferred.

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **BE-03** in your repository README and in your reply email.

## Context
Before approving a transaction, we run fraud and risk rules. They must be fast (under 50 ms) because they sit on the payment path.

## Requirements

- `POST /risk/check`
  - Body: `{ "userId": "...", "amount": 2000, "deviceId": "...", "ip": "...", "merchantCategory": "GAMING" }`
  - Response: `{ "decision": "ALLOW" | "REVIEW" | "BLOCK", "reasons": ["..."] }`
- Implement at least these rules:
  1. More than 5 transactions from the same user within 1 minute → `BLOCK`.
  2. Total amount over ₹1,00,000 for a user in 24 hours → `REVIEW`.
  3. A new device seen for the user in the last 10 minutes and amount over ₹10,000 → `REVIEW`.
  4. Merchant category on a configurable blocklist → `BLOCK`.
- Rules must be **configurable** (limits, time windows) without code changes: JSON config, a DB table, or an admin endpoint.
- Use Redis (or an in-memory equivalent with a clear interface) for counters and time windows.
- `GET /risk/decisions?userId=`: audit trail of past decisions.
- Rate-limit the `/risk/check` endpoint per API key.

## Bonus
- Unit tests for each rule, including the edge of each time window.
- A short note on how you would scale this to 5,000 checks per second.
