# FS-02: Merchant Payment Links

**Role:** Full Stack Engineer (MERN) · **Time:** about 3 hours (additional tasks are optional)

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **FS-02** in your repository README and in your reply email.

## Scenario

A merchant creates a payment link and shares it with a customer. The payment result arrives later from a payment provider through a webhook, sometimes more than once.

## Must have

**Backend (Node.js + Express + MongoDB)**

- One hard-coded merchant (no login needed).
- `POST /payment-links`: create a link `{ amount, description }`. Status starts as `ACTIVE`.
- `POST /pay/:linkId`: the customer starts a payment. Creates a payment with status `PENDING`. A link can be paid **only once**.
- `POST /webhooks/provider` with body `{ "eventId": "...", "paymentId": "...", "status": "SUCCESS" | "FAILED" }`
  - the same `eventId` arriving twice is processed only once
  - a final status (`SUCCESS` / `FAILED`) never changes back
  - on `SUCCESS`, the link becomes `PAID`

**Frontend (React)**

- Merchant page: create a link and see all links with their status.
- Customer page `/pay/:linkId`: amount, description and a "Pay" button.

Send webhooks with curl or Postman. Include example commands in your README.

## Additional tasks (optional, plus points)

Not required. Each one you complete counts in your favour.

- Merchant login with JWT, so only the merchant can create and see their links.
- Add MongoDB indexes for your main queries and explain each one in your README.
- Link expiry.
- Verify a shared secret in a webhook header.
- Auto-refresh of link status on the merchant page.
- Tests for duplicate and out-of-order webhooks.

## Answer in your README (a few lines each)

1. The provider sends `FAILED` after `SUCCESS` for the same payment. What does your system do, and why?
2. How would you make sure a link is never paid twice if two customers press "Pay" at the same moment?

If you run out of time, list what you skipped and how you would build it.
