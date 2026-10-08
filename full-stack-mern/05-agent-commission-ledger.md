# FS-05: Agent Commission Ledger

**Role:** Full Stack Engineer (MERN) · **Time:** about 3 hours (additional tasks are optional)

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **FS-05** in your repository README and in your reply email.

## Scenario

In mobile money, **distributors** manage a network of **agents**. Each time an agent completes a cash-in for a customer, a commission is earned and split between the agent and their distributor. Commissions are money, so every paisa must add up.

## Must have

All amounts in the API are **integers in paise** (for example, `250000` = ₹2,500).

**Backend (Node.js with Express or NestJS, MongoDB)**

- Seed 2 `distributor` users and 3 `agent` users, each agent linked to one distributor. A simple JWT login is enough.
- `POST /cash-ins` (agent only) with body `{ "customerMobile": "...", "amount": 250000 }` and header `Idempotency-Key`
  - total commission = **1% of the amount**, capped at ₹50
  - split: **70% to the agent, 30% to the distributor**
  - the two shares must always add up exactly to the total commission; explain how you handle rounding
  - the same `Idempotency-Key` never creates a second cash-in or commission
- `GET /commissions/summary?period=today|month`
  - an agent sees only their own earnings
  - a distributor sees their own earnings plus a per-agent breakdown of their agents
- Role checks on every endpoint (an agent cannot read another agent's data).

**Frontend (React or Next.js)**

- Login, then a dashboard that depends on the role:
  - agent: a cash-in form and their commission totals
  - distributor: their totals plus a table of their agents
- Period switch (today / this month) and loading, empty and error states.

## Additional tasks (optional, plus points)

Not required. Each one you complete counts in your favour.

- An `admin` role that sees totals for all distributors.
- A "this week" period.
- Commission rules (percentage, cap, split) from config, not hard-coded.
- Tests for the commission and rounding rules, and for role access.
- MongoDB aggregation for the summary, with indexes explained.
- TypeScript and TanStack Query.

## Answer in your README (a few lines each)

1. A cash-in of ₹333 gives a commission of ₹3.33. How exactly is it split, and where does the leftover paisa go?
2. How did you make sure an agent can never see another agent's data, even by changing an ID in the URL?

If you run out of time, list what you skipped and how you would build it.
