# NET-01: Wallet API — Interviewer notes

> **Private. Do not share with candidates or push to GitHub.**
> Assessment sent to the candidate: [01-wallet-api.md](01-wallet-api.md)

## Call plan (30–40 min)

1. **Demo (5 min):** the candidate shares their screen and runs the project.
2. **Cross-questions (15 min):** the questions below. Ask them to open the exact file or line.
3. **Live change (10 min):** see below. AI tools are allowed, but they must explain what they did.
4. **README answers (5 min):** ask them to explain their written answers in their own words.

## Cross-questions

1. Show me the transaction around the debit and credit. Which isolation level does it use, and why?
2. Where is the `idempotencyKey` stored? Is there a unique constraint, and what happens when it is violated?
3. Two transfers from the same wallet run at the same moment. Walk me through your code line by line.
4. Show me how the `DbContext` is registered. Which lifetime, and what would break if it were a Singleton?
5. Find any `.Result` or `.Wait()` in your code. Why is it a problem?

## Live change

Add an admin-only endpoint that reverses a transfer, without allowing it twice.

## Their README questions (ask them to explain)

1. What happens if two transfers from the same wallet run at the same moment? How did you handle it?
2. Why did you choose EF Core or Dapper for this task?

## General questions

- Which part was hardest, and how did you solve it?
- Which part did AI help with most? What did you change in what it gave you?
- If you had 2 more hours, what would you add first, and why?

## Red flags

- Cannot find where a rule is handled in their own code.
- Reads code aloud line by line instead of explaining why.
- Claims everything is handled but cannot show where.
- Cannot make the live change without rewriting large parts.

## Score (1–4)

| Area | Score | Notes |
| --- | --- | --- |
| Submission quality (Must have) |  |  |
| Additional tasks (plus points) |  |  |
| Understanding of own code |  |  |
| Live change |  |  |
| Failure / fintech thinking |  |  |
| Communication |  |  |

Candidate: ______ · Interviewer: ______ · Date: ______ · Recommendation: Hire / Second opinion / No hire
