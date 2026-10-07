# NET-03: Payout API with Maker-Checker Approval

**Role:** .NET Developer  
**Stack:** C#, ASP.NET Core Web API (.NET 8 preferred), Entity Framework Core or Dapper, SQL Server or MySQL.

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **NET-03** in your repository README and in your reply email.

## Context
Large payouts to merchants need two people: a **maker** creates the payout and a different **checker** approves it. This is a standard control in banking.

## Requirements

- Roles: `Maker`, `Checker`, `Admin` (JWT, policy-based authorization).
- `POST /api/payouts`: maker creates a payout `{ merchantId, amount, bankAccount, reason }`.
  - Payouts above ₹1,00,000 need checker approval. Smaller ones are auto-approved.
- `POST /api/payouts/{id}/approve` and `/reject`: checker only. **The maker cannot approve their own payout.**
- Statuses: `PENDING_APPROVAL → APPROVED → PROCESSING → PAID | FAILED`, or `REJECTED`. Block invalid transitions.
- Approved payouts are sent to a **mock bank API** (write it yourself) that randomly succeeds or fails. Failed payouts can be retried without paying twice.
- Every action writes an **audit log** (user, action, old status, new status, timestamp, IP).
- `GET /api/payouts` with filters (status, merchant, date range, amount range) and paging.
- Mask bank account numbers in all responses (`XXXXXX4321`).

## Bonus
- Integration tests for the maker-checker rule and the retry behaviour.
- Use the outbox pattern or a queue (RabbitMQ / Kafka) for sending payouts. Explain why.
