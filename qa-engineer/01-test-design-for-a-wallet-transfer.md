# QA-01: Test Design for a Wallet Transfer

**Role:** QA Engineer / Software Tester  

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **QA-01** in your repository README and in your reply email.

## Requirement under test

> A logged-in user can send money from their wallet to another registered user by entering the receiver's mobile number and an amount.
> - Amount must be between ₹1 and ₹50,000 per transaction, with a daily limit of ₹1,00,000.
> - The sender must have enough balance.
> - The sender confirms with a 4-digit PIN. Three wrong PINs lock transfers for 30 minutes.
> - On success, both users receive an SMS and see the transaction in their history.
> - If the network fails, the app retries the request once automatically.

## Deliverables

1. **Test plan (1–2 pages):** scope, out of scope, test types, environments, entry and exit criteria, risks.
2. **Test cases (at least 30)** in a table: ID, title, preconditions, steps, test data, expected result, priority, type (positive / negative / boundary / security / failure).
   Must cover: amount boundaries, daily limit, balance, PIN lockout, duplicate submission, network failure and retry, SMS failure, concurrent transfers, and receiver's balance.
3. **SQL queries** (assume tables `users`, `wallets`, `transactions` and design their columns yourself):
   - find transactions stuck in `PENDING` for more than 30 minutes
   - find users whose wallet balance does not equal the sum of their successful credits minus debits
   - find duplicate transactions with the same sender, receiver and amount within 1 minute
4. **Three bug reports** for these observed problems:
   - money debited twice when the user tapped "Send" twice on a slow network
   - the receiver got the SMS but the amount does not appear in their history
   - the daily limit resets at 12:00 noon instead of midnight

## Bonus
- A risk-based list: if you had only 2 hours before release, which 10 test cases would you run and why?
