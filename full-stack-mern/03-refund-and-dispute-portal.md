# FS-03: Refund Portal

**Role:** Full Stack Engineer (MERN) · **Time:** about 3 hours (bonus is optional)

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **FS-03** in your repository README and in your reply email.

## Scenario

Our operations team handles customer refund requests. A refund must never exceed the original payment, and the person who requests a refund cannot approve it.

## Must have

**Backend (Node.js + Express + MongoDB)**

- Seed about 20 payments (id, customer, amount, status) and two users: an `agent` and a `supervisor`. Pick the user with a simple header or login.
- `POST /payments/:id/refunds`: the agent requests a full or partial refund.
  - only `SUCCESS` payments can be refunded
  - approved + pending refunds must never exceed the payment amount
- `POST /refunds/:id/approve` and `/reject`: supervisor only.
- `GET /payments/:id`: payment details with its refunds.

**Frontend (React)**

- Payments list.
- Payment detail page with refund history and a "Request refund" form.
- Pending refunds list with Approve / Reject buttons for the supervisor.

## Bonus (optional)

- Audit log (who did what, when).
- Search and filters on the payments list.
- Tests for the refund limit and role rules.

## Answer in your README (a few lines each)

1. Two agents request a refund on the same payment at the same moment. How do you stop the total going over the limit?
2. What would you add before this went to production?

If you run out of time, list what you skipped and how you would build it.
