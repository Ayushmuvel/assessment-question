# NET-03: Payout Approval API (Maker-Checker)

**Role:** .NET Developer · **Time:** about 3 hours (additional tasks are optional)

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **NET-03** in your repository README and in your reply email.

## Scenario

Large payouts to merchants need two people: a **maker** creates the payout and a different **checker** approves it. This is a standard banking control.

## Must have

- ASP.NET Core Web API with EF Core or Dapper. SQL Server, MySQL or SQLite.
- JWT with seeded users in two roles: `Maker` and `Checker`.
- `POST /api/payouts`: a maker creates `{ merchantId, amount, reason }`.
  - above ₹1,00,000 → status `PENDING_APPROVAL`
  - otherwise → `APPROVED`
- `POST /api/payouts/{id}/approve` and `/reject`: checker only. **The maker cannot approve their own payout**, even with both roles.
- Block invalid status changes (for example, approving a `REJECTED` payout).
- `GET /api/payouts?status=`: list with a filter.
- Swagger enabled.

## Additional tasks (optional, plus points)

Not required. Each one you complete counts in your favour.

- Use MySQL or SQL Server instead of SQLite.
- Audit log (user, action, old status, new status, time).
- Send approved payouts to a mock bank that can fail, with a retry that never pays twice.
- Integration tests for the maker-checker rule.

## Answer in your README (a few lines each)

1. Two checkers approve the same payout at the same moment. What happens?
2. Where did you put the maker-checker rule (controller, service, policy), and why?

If you run out of time, list what you skipped and how you would build it.
