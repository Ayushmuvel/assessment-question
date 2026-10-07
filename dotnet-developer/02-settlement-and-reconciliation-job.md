# NET-02: Settlement Reconciliation

**Role:** .NET Developer · **Time:** about 3 hours (additional tasks are optional)

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **NET-02** in your repository README and in your reply email.

## Scenario

Every night, our partner bank sends a settlement file of the payments it processed. We match it against our own records and report differences before paying merchants.

## Must have

- ASP.NET Core Web API with EF Core or Dapper. SQL Server, MySQL or SQLite.
- Seed about 50 transactions (`TransactionId`, `Amount`, `Status`).
- `POST /api/settlements/upload`: upload a CSV (`transaction_id,amount`). Include a sample file with a few deliberate problems.
- Return a reconciliation result with four groups:
  - matched
  - missing in the bank file
  - missing in our system
  - amount mismatch
- Uploading the **same file twice** does not create duplicate results.
- Swagger enabled.

## Additional tasks (optional, plus points)

Not required. Each one you complete counts in your favour.

- JWT authentication, with uploads allowed only for an `Admin` role.
- Use MySQL or SQL Server instead of SQLite.
- Process the file in a background service and expose `GET /api/settlements/{id}` for status.
- Detect duplicate rows inside the bank file.
- Unit tests for the matching logic.

## Answer in your README (a few lines each)

1. How would you handle a file with 1 million rows?
2. How did you detect that the same file was uploaded twice?

If you run out of time, list what you skipped and how you would build it.
