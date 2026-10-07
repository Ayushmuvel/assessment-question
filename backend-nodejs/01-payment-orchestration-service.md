# BE-01: Payment Status Service

**Role:** Backend Engineer (Node.js) · **Time:** about 3 hours (additional tasks are optional)

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **BE-01** in your repository README and in your reply email.

## Scenario

Our platform creates payments and receives status updates from a payment provider by webhook. Webhooks can arrive twice or out of order.

## Must have

- `POST /payments` with body `{ "orderId": "ORD-1001", "amount": 1500 }`
  - creates a payment with status `PENDING`
  - the same `orderId` never creates a second payment; a repeat request returns the existing one
- `POST /webhooks/provider` with body `{ "eventId": "...", "paymentId": "...", "status": "SUCCESS" | "FAILED" }`
  - each `eventId` is processed once
  - allowed moves: `PENDING → SUCCESS` and `PENDING → FAILED`; anything else is ignored and logged
- `GET /payments/:id`: current status and status history.
- Any database (MongoDB, PostgreSQL, MySQL or SQLite).
- Include a Postman collection or curl examples.

## Additional tasks (optional, plus points)

Not required. Each one you complete counts in your favour.

- Use MongoDB with indexes that enforce the duplicate rules (`orderId`, `eventId`).
- A job that marks payments still `PENDING` after 15 minutes as `EXPIRED`.
- Verify an HMAC signature on the webhook.
- Tests for the duplicate `orderId` and the duplicate webhook.

## Answer in your README (a few lines each)

1. Your service runs on two servers and both receive the same webhook at the same moment. What happens?
2. Calling the provider times out. Should you retry? How do you avoid creating two payments?

If you run out of time, list what you skipped and how you would build it.
