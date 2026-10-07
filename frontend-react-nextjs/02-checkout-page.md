# FE-02: Checkout Page

**Role:** Frontend Engineer (React / Next.js)  
**Stack:** Next.js (App Router preferred), React, TypeScript, Tailwind CSS (or a comparable styling approach).

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **FE-02** in your repository README and in your reply email.

## Context
A customer arrives on our hosted checkout page from a merchant's website to pay for an order. The page must be fast, trustworthy, and must never let the customer pay twice.

## Requirements

- Route `/checkout/[orderId]` loads order details (merchant name, items, total) from a mock API.
- Payment methods as tabs: **UPI** (enter UPI ID, validate format), **Card** (number with Luhn check, expiry, CVV, masked display) and **Wallet**.
- After "Pay":
  - Show an OTP step for cards (mock OTP `123456`).
  - Show a "Processing" screen that polls the payment status.
  - Handle the outcomes `SUCCESS`, `FAILED` (with retry) and `TIMEOUT` (show "We are confirming your payment", do **not** offer to pay again).
- The Pay button can never trigger two payments: disable it and send an idempotency key with the request.
- If the user refreshes during processing, the page resumes the status check instead of starting a new payment.
- Accessible: full keyboard navigation, labelled inputs, visible focus, readable error messages.
- Server-render the order summary for fast first paint.

## Bonus
- Session expiry timer (for example, 10 minutes) with a clear message when it runs out.
- Playwright test covering the double-click and refresh cases.
