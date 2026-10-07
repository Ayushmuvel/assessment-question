# BE-03: Transaction Risk Checks

**Role:** Backend Engineer (Node.js) · **Time:** about 3 hours (bonus is optional)

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **BE-03** in your repository README and in your reply email.

## Scenario

Before approving a transaction, we run fraud rules. They sit on the payment path, so they must be fast.

## Must have

- `POST /risk/check` with body `{ "userId": "...", "amount": 2000, "merchantCategory": "GAMING" }`
  - returns `{ "decision": "ALLOW" | "REVIEW" | "BLOCK", "reasons": ["..."] }`
- Implement these rules:
  1. more than 5 checks for the same user within 1 minute → `BLOCK`
  2. total amount over ₹1,00,000 for a user in the last 24 hours → `REVIEW`
  3. merchant category on a blocklist → `BLOCK`
- Limits, time windows and the blocklist come from a config file, not hard-coded values.
- Store counters in Redis, or in memory behind a clear interface that could be swapped for Redis.
- Unit tests for each rule.

## Bonus (optional)

- `GET /risk/decisions?userId=`: history of past decisions.
- Test the edge of each time window (exactly 1 minute, exactly 24 hours).

## Answer in your README (a few lines each)

1. How would this work with 5 servers instead of one?
2. If Redis is down, should you allow or block transactions? Why?

If you run out of time, list what you skipped and how you would build it.
