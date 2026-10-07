# QA-01: Test Design for a Wallet Transfer

**Role:** QA Engineer / Software Tester · **Time:** about 3 hours (additional tasks are optional)

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **QA-01** in your repository README and in your reply email.

## Requirement under test

> A logged-in user can send money to another registered user by entering the receiver's mobile number and an amount.
> - Amount must be between ₹1 and ₹50,000 per transaction, with a daily limit of ₹1,00,000.
> - The sender must have enough balance.
> - The sender confirms with a 4-digit PIN. Three wrong PINs lock transfers for 30 minutes.
> - On success, both users receive an SMS and see the transaction in their history.
> - If the network fails, the app retries the request once automatically.

## Must have

1. **Test cases (at least 25)** in a table: ID, title, steps, test data, expected result, priority, and type (positive / negative / boundary / failure).
   Cover: amount boundaries, daily limit, balance, PIN lockout, double tap on Send, network failure and retry, and the receiver's balance.
2. **Two SQL queries.** Assume tables `users`, `wallets` and `transactions`, and describe the columns you assume.
   - transactions stuck in `PENDING` for more than 30 minutes
   - duplicate transactions with the same sender, receiver and amount within 1 minute
3. **Two bug reports:**
   - money was debited twice when the user tapped "Send" twice on a slow network
   - the daily limit resets at 12:00 noon instead of midnight

Submit as Markdown, a spreadsheet (`.xlsx` / `.csv`) or PDF.

## Additional tasks (optional, plus points)

Not required. Each one you complete counts in your favour.

- 5 refund test cases: full, partial, more than the payment amount, duplicate refund request, refund of a failed payment.
- 5 mobile-specific test cases: incoming call during payment, app sent to background or killed, poor network, permission denied, small and large screens.
- A short test plan: scope, out of scope, risks, entry and exit criteria.
- If you had only 1 hour before release, which 10 test cases would you run and why?

## Answer in your README (a few lines each)

1. Which single test case here protects the company most, and why?
2. How would you test the "retry once" behaviour without a real network failure?
