# NET-01: Wallet API

**Role:** .NET Developer · **Time:** about 3 hours (bonus is optional)

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **NET-01** in your repository README and in your reply email.

## Scenario

A digital wallet lets users transfer money. Clients retry on timeouts, so transfers must be safe to call twice.

## Must have

- ASP.NET Core Web API with EF Core or Dapper. SQL Server, MySQL or SQLite.
- Seed 3 users with a balance. JWT login (a simple seeded user list is fine).
- `POST /api/wallet/transfer` with body `{ "toUserId": 2, "amount": 500.00, "idempotencyKey": "..." }`
  - amount between ₹1 and ₹50,000, no self-transfer, enough balance required
  - debit and credit in **one database transaction**
  - the same `idempotencyKey` returns the original result without moving money again
- `GET /api/transactions?page=1&pageSize=20`: paged history for the logged-in user.
- Global exception handling with a consistent error response.
- Controller → Service → Repository layers, using dependency injection and DTOs.
- Swagger enabled.

## Bonus (optional)

- xUnit tests for the transfer service.
- An admin-only endpoint for monthly totals per user, using an optimised SQL query.

## Answer in your README (a few lines each)

1. What happens if two transfers from the same wallet run at the same moment? How did you handle it?
2. Why did you choose EF Core or Dapper for this task?

If you run out of time, list what you skipped and how you would build it.
