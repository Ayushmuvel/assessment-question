# FS-02: Merchant Payment Links

**Role:** Full Stack Engineer (MERN)  
**Stack:** MongoDB, Express.js, React, Node.js. TypeScript is preferred.

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **FS-02** in your repository README and in your reply email.

## Context
A small business creates a payment link, shares it with a customer, and gets paid. Payment status arrives later from a payment provider through a webhook.

## Requirements

**Backend**

- Merchant login (JWT).
- `POST /payment-links`: create a link with `{ amount, description, expiresInMinutes }`. Returns a public URL such as `/pay/:linkId`.
- `GET /pay/:linkId`: public endpoint returning link details (amount, merchant name, status).
- `POST /pay/:linkId/checkout`: simulate starting a payment. Create a payment record with status `PENDING`.
- `POST /webhooks/provider`: simulated provider callback with `{ paymentId, status: "SUCCESS" | "FAILED", eventId }`.
  - The same `eventId` can arrive more than once. Process it only once.
  - A final status (`SUCCESS` or `FAILED`) must never change back to `PENDING`.
  - Verify a shared secret sent in a header (for example, `X-Signature`).
- A link can be paid **only once**. Expired links cannot be paid.

**Frontend**

- Merchant dashboard: create a link, copy it, see all links with status (`ACTIVE`, `PAID`, `EXPIRED`).
- Public checkout page for the customer at `/pay/:linkId` with a "Pay" button.
- A small "Provider simulator" page or script that sends webhook calls (success, failure, duplicate).

## Bonus
- Real-time status update on the dashboard (polling or WebSocket).
- Tests covering duplicate webhooks and paying an expired link.
