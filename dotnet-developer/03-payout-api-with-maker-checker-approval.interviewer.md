# NET-03: Payout Approval API (Maker-Checker) — Interviewer notes

> **Private. Do not share with candidates or push to GitHub.**
> Assessment sent to the candidate: [03-payout-api-with-maker-checker-approval.md](03-payout-api-with-maker-checker-approval.md)

## Call plan (30–40 min)

1. **Demo (5 min):** the candidate shares their screen and runs the project.
2. **Cross-questions (15 min):** the questions below. Ask them to open the exact file or line.
3. **Live change (10 min):** see below. AI tools are allowed, but they must explain what they did.
4. **README answers (5 min):** ask them to explain their written answers in their own words.

## Cross-questions

1. Show me where the "maker cannot approve their own payout" rule lives. Could a user with both roles get around it?
2. Show me how invalid status changes are blocked. Is it a state machine, if-statements, or something else?
3. Two checkers approve the same payout at the same moment. What stops a double approval? (row version, concurrency token, conditional update)
4. How are roles in the JWT checked: `[Authorize(Roles)]` or policies? Why?
5. What does a payout response contain? Is any sensitive data exposed?

## Live change

Payouts over ₹10,00,000 need approval from two different checkers.

## Their README questions (ask them to explain)

1. Two checkers approve the same payout at the same moment. What happens?
2. Where did you put the maker-checker rule (controller, service, policy), and why?

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
