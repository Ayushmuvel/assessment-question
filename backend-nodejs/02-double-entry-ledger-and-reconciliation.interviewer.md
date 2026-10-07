# BE-02: Double-Entry Ledger — Interviewer notes

> **Private. Do not share with candidates or push to GitHub.**
> Assessment sent to the candidate: [02-double-entry-ledger-and-reconciliation.md](02-double-entry-ledger-and-reconciliation.md)

## Call plan (30–40 min)

1. **Demo (5 min):** the candidate shares their screen and runs the project.
2. **Cross-questions (15 min):** the questions below. Ask them to open the exact file or line.
3. **Live change (10 min):** see below. AI tools are allowed, but they must explain what they did.
4. **README answers (5 min):** ask them to explain their written answers in their own words.

## Cross-questions

1. Show me the transaction that writes the debit and the credit. What happens if the credit insert fails?
2. How do you guarantee the account never goes below zero with 10 parallel transfers? Show me the line.
3. How are amounts stored? Show me converting ₹12.34 into your format and back.
4. Sum all entries in the ledger. What should the total be, and why?
5. The balance calculation becomes slow with 10 million entries. What would you change?

## Live change

Add a fee: each transfer also credits a `FEES` account with ₹2, still balanced.

## Their README questions (ask them to explain)

1. Why calculate the balance from entries instead of storing one number? What would you do when it gets slow?
2. How did you prevent two parallel transfers from overdrawing an account?

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
