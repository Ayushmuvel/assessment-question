# FS-04: Wallet Top-Up via Payment Gateway

**Role:** Full Stack Engineer (MERN) · **Time:** about 3 hours (additional tasks are optional)

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **FS-04** in your repository README and in your reply email.

## Scenario

Users add money to their wallet through a payment gateway. The user is sent to the gateway, pays, and comes back. The real result arrives separately through a webhook, which can come late, twice, or after the user has returned. The wallet must be credited exactly once.

## Must have

All amounts in the API are **integers in paise** (for example, `100000` = ₹1,000).

**Backend (Node.js with Express or NestJS, MongoDB)**

- Seed 2 users. A simple JWT login is enough.
- `POST /topups` with body `{ "amount": 100000 }`: amount between ₹100 and ₹50,000. Creates a top-up order with status `PENDING` and returns a `gatewayUrl` pointing to your mock gateway page.
- **Mock gateway page** (served by your app): shows the amount with "Pay" and "Fail" buttons. Each button redirects the user back to the app **immediately**, and sends the webhook to your backend **about 5 seconds later** (to simulate a slow gateway).
- `POST /webhooks/gateway` with body `{ "eventId": "...", "orderId": "...", "status": "SUCCESS" | "FAILED" }`
  - the wallet is credited **only once per order**, even if the webhook arrives twice
  - a final status (`SUCCESS` / `FAILED`) never changes
- `GET /topups/:id`: order status, used by the frontend after the redirect.
- `GET /wallet`: current balance.

**Frontend (React or Next.js)**

- Wallet page: balance, an "Add money" form and a list of top-ups with status.
- After returning from the gateway, show "Confirming payment…" and check the status until it is final, then refresh the balance.

Include a curl or Postman example in your README that re-sends a webhook, so we can test the duplicate case.

## Additional tasks (optional, plus points)

Not required. Each one you complete counts in your favour.

- Real-time balance update with Socket.io instead of status checks.
- Verify an HMAC signature on the webhook.
- A test proving that a duplicate webhook credits the wallet only once.
- TypeScript on both backend and frontend.
- MongoDB indexes that enforce the duplicate rules, with a reason for each.

## Answer in your README (a few lines each)

1. The user comes back from the gateway before the webhook arrives. What does the user see, and why is it safe?
2. Why should you never credit the wallet based on the browser redirect alone?

If you run out of time, list what you skipped and how you would build it.
