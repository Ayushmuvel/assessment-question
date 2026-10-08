# FS-02: Merchant Payment Links

**Role:** Full Stack Engineer (MERN) · **Time:** about 3 hours (additional tasks are optional)

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **FS-02** in your repository README and in your reply email.

## Scenario

A merchant creates a payment link and shares it with a customer. The payment result arrives later from a payment provider through a webhook, sometimes more than once.

## Must have

All amounts in the API are **integers in paise** (for example, `50000` = ₹500).

**Backend (Node.js with Express or NestJS, MongoDB)**

- One hard-coded merchant (no login needed).
- `POST /payment-links`: create a link `{ amount, description }`. Status starts as `ACTIVE`.
- `GET /payment-links`: all links with their status (for the merchant page).
- `GET /pay/:linkId`: link details and the status of its latest payment (for the customer page).
- `POST /pay/:linkId`: the customer starts a payment, creating a payment with status `PENDING`.
  - if a payment for this link is already `PENDING`, return that payment instead of creating a new one
  - after a `FAILED` payment, the customer may try again
  - once a payment succeeds, the link is `PAID` and cannot be paid again
- `POST /webhooks/provider` with body `{ "eventId": "...", "paymentId": "...", "status": "SUCCESS" | "FAILED" }`
  - the same `eventId` arriving twice is processed only once
  - a final status (`SUCCESS` / `FAILED`) never changes back
  - on `SUCCESS`, the link becomes `PAID`

**Frontend (React or Next.js)**

- Merchant page: create a link and see all links with their status.
- Customer page `/pay/:linkId`: amount, description and a "Pay" button. After paying, show "Waiting for confirmation…" and update to the result when the webhook arrives (checking the status every few seconds is fine).

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
