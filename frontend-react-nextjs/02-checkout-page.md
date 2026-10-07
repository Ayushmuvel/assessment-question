# FE-02: Checkout Page

**Role:** Frontend Engineer (React / Next.js) · **Time:** about 3 hours (additional tasks are optional)

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **FE-02** in your repository README and in your reply email.

## Scenario

A customer lands on our checkout page to pay for an order. The page must never let them pay twice, even on a slow network.

## Must have

- Next.js + TypeScript.
- Route `/checkout/[orderId]` shows an order summary (merchant, amount) from mock data.
- Two payment methods:
  - **UPI ID** with format validation (for example `name@bank`)
  - **Card** with a Luhn check on the number, plus expiry and CVV validation
- After "Pay", a mock API returns `SUCCESS`, `FAILED` or `TIMEOUT` at random.
  - Show a processing state, then the result screen.
  - On `FAILED`, allow retry. On `TIMEOUT`, show "We are confirming your payment" and do **not** offer to pay again.
- The Pay button can never send two payments: disable it, and send the same idempotency key with the request.
- Labelled inputs, keyboard usable, clear error messages.

## Additional tasks (optional, plus points)

Not required. Each one you complete counts in your favour.

- Unit tests for the Luhn and UPI validators.
- Refreshing during processing resumes the status check instead of starting a new payment.
- Card OTP step (mock OTP `123456`).
- A Playwright test for the double-click case.

## Answer in your README (a few lines each)

1. Why is disabling the button not enough on its own to prevent double payments?
2. What should the page show if the user closes the tab during processing and comes back?

If you run out of time, list what you skipped and how you would build it.
