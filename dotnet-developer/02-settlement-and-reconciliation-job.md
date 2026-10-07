# NET-02: Settlement & Reconciliation Job

**Role:** .NET Developer  
**Stack:** C#, ASP.NET Core Web API (.NET 8 preferred), Entity Framework Core or Dapper, SQL Server or MySQL.

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **NET-02** in your repository README and in your reply email.

## Context
Every night, our partner bank sends a settlement file listing the payments it processed. We must match it against our own transaction records and report any differences before money is paid out to merchants.

## Requirements

- Seed your database with at least 200 transactions (`TransactionId`, `MerchantId`, `Amount`, `Status`, `CreatedAt`).
- `POST /api/settlements/upload`: upload a CSV file (`bank_ref,transaction_id,amount,settled_at`). Include a sample file with some deliberate problems.
- Process the file in a **background service** (`BackgroundService` / hosted service or a queue), not inside the HTTP request.
- Produce a reconciliation result:
  - matched
  - missing in bank file
  - missing in our system
  - amount mismatch
  - duplicate rows in the bank file
- `GET /api/settlements/{id}`: processing status and summary counts.
- `GET /api/settlements/{id}/exceptions`: the unmatched rows, paged.
- Uploading the **same file twice** must not create duplicate results.
- Per-merchant settlement summary: total settled amount minus a configurable fee percentage.

## Bonus
- Handle a 100,000-row file efficiently (streaming, batching, bulk insert). Explain your approach.
- Unit tests for the matching logic.
