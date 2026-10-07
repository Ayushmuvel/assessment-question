# FS-03: Refund & Dispute Portal

**Role:** Full Stack Engineer (MERN)  
**Stack:** MongoDB, Express.js, React, Node.js. TypeScript is preferred.

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **FS-03** in your repository README and in your reply email.

## Context
Our operations team handles customer refund requests. Refunds must never exceed the original payment, and every action must be auditable.

## Requirements

**Backend**

- Seed the database with at least 30 sample payments (amount, customer, merchant, status, date).
- Roles: `agent` (can request refunds) and `supervisor` (can approve or reject them).
- `POST /payments/:id/refunds`: agent requests a full or partial refund with a reason.
  - The total of approved + pending refunds must never exceed the payment amount.
  - Only `SUCCESS` payments can be refunded.
- `POST /refunds/:id/approve` and `POST /refunds/:id/reject`: supervisor only. An agent cannot approve their own request.
- Every create, approve and reject writes an **audit log** entry (who, what, when, old value, new value).
- `GET /payments` with search (customer name or payment ID), filters (status, date range) and pagination.

**Frontend**

- Payments table with search, filters and pagination.
- Payment detail page: payment info, refund history, "Request refund" form.
- Supervisor view: a queue of pending refunds with Approve / Reject actions.
- Audit log view for a payment.

## Bonus
- Concurrency safety: two agents requesting refunds on the same payment at the same time cannot exceed the limit.
- Tests for the refund limit rules and role permissions.
