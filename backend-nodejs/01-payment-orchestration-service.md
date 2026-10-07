# BE-01: Payment Orchestration Service

**Role:** Backend Engineer (Node.js)  
**Stack:** Node.js with Express or NestJS. MongoDB, PostgreSQL or MySQL. TypeScript is preferred.

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **BE-01** in your repository README and in your reply email.

## Context
Our platform sends payments to external payment providers. Providers are slow, sometimes time out, and send status updates by webhook, sometimes twice and sometimes out of order.

## Requirements

- `POST /payments`
  - Body: `{ "orderId": "ORD-1001", "amount": 1500, "currency": "INR", "method": "UPI" }`
  - The same `orderId` must never create two payments. A repeat request returns the existing payment.
  - Calls a **mock provider** (write it yourself, in-process or as a separate service) that randomly succeeds, fails or times out.
- Payment states: `CREATED → PENDING → SUCCESS | FAILED`, plus `EXPIRED`. Reject invalid transitions (for example, `SUCCESS → PENDING`).
- `POST /webhooks/provider`
  - Body: `{ "eventId": "...", "paymentId": "...", "status": "SUCCESS" }`
  - Verify an HMAC signature header.
  - Process each `eventId` once. Ignore an event that would move a final state backwards.
- `GET /payments/:id`: current status and status history.
- A background job marks payments still `PENDING` after 15 minutes as `EXPIRED`. It must not double-process when two instances of the service run.

## Bonus
- Retries with exponential backoff when the provider times out, without creating duplicate provider payments.
- Integration tests for: duplicate `orderId`, duplicate webhook, out-of-order webhook.
- Docker Compose setup.
