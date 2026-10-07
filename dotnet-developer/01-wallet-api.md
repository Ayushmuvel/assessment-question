# NET-01: Wallet API

**Role:** .NET Developer  
**Stack:** C#, ASP.NET Core Web API (.NET 8 preferred), Entity Framework Core or Dapper, SQL Server or MySQL.

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **NET-01** in your repository README and in your reply email.

## Context
A digital wallet lets users top up, pay and transfer money. Clients retry on timeouts, so every money-moving endpoint must be safe to call twice.

## Requirements

- JWT authentication with roles `User` and `Admin`.
- `POST /api/wallet/topup`: add money (simulate a successful bank top-up).
- `POST /api/wallet/transfer`
  - Body: `{ "toUserId": 12, "amount": 500.00, "idempotencyKey": "..." }`
  - Amount between ₹1 and ₹50,000; no self-transfer; enough balance required.
  - Debit and credit happen in **one database transaction**.
  - The same `idempotencyKey` returns the original result without moving money again.
- `GET /api/transactions?page=1&pageSize=20&type=&status=`: paged history.
- `GET /api/admin/reports/monthly?year=2026&month=10`: per-user totals (top-ups, sent, received) using a **stored procedure or an optimised SQL query**.
- Global exception middleware returning a consistent error format (`code`, `message`, `traceId`).
- Layered design: Controllers → Services → Repositories, with DTOs and dependency injection.

## Bonus
- xUnit tests for the transfer service (insufficient balance, duplicate key, concurrency).
- Redis caching for the balance with correct invalidation.
- Docker Compose (API + database).
