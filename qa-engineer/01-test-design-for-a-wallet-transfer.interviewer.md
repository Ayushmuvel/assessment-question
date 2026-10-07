# QA-01: Test Design for a Wallet Transfer — Interviewer notes

> **Private. Do not share with candidates or push to GitHub.**
> Assessment sent to the candidate: [01-test-design-for-a-wallet-transfer.md](01-test-design-for-a-wallet-transfer.md)

## Call plan (30–40 min)

1. **Walkthrough (5 min):** the candidate shares their screen and walks through what they submitted.
2. **Cross-questions (15 min):** the questions below. Ask them to open the exact file or line.
3. **Live exercise (10 min):** see below. AI tools are allowed, but they must explain what they did.
4. **README answers (5 min):** ask them to explain their written answers in their own words.

## Cross-questions

1. Pick your 3 highest-priority test cases and explain why each is high priority.
2. Read me your boundary cases for the amount. Why those exact values?
3. How would you actually execute the "double tap on Send" case? What evidence would you collect?
4. Walk me through your duplicate-transaction SQL. What happens with three identical transfers in 1 minute? With one at 0:59 and one at 1:01?
5. In your double-debit bug report, why did you choose that severity and priority?

## Live exercise

"The product team adds a rule: transfers over ₹10,000 need an OTP." Write 5 new test cases on the spot.

## Their README questions (ask them to explain)

1. Which single test case here protects the company most, and why?
2. How would you test the "retry once" behaviour without a real network failure?

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
| Live exercise |  |  |
| Failure / fintech thinking |  |  |
| Communication |  |  |

Candidate: ______ · Interviewer: ______ · Date: ______ · Recommendation: Hire / Second opinion / No hire
