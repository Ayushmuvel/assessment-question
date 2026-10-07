# FS-03: Refund Portal — Interviewer notes

> **Private. Do not share with candidates or push to GitHub.**
> Assessment sent to the candidate: [03-refund-and-dispute-portal.md](03-refund-and-dispute-portal.md)

## Call plan (30–40 min)

1. **Demo (5 min):** the candidate shares their screen and runs the project.
2. **Cross-questions (15 min):** the questions below. Ask them to open the exact file or line.
3. **Live change (10 min):** see below. AI tools are allowed, but they must explain what they did.
4. **README answers (5 min):** ask them to explain their written answers in their own words.

## Cross-questions

1. Show me the refund limit check. Does it count pending refunds as well as approved ones? Why?
2. Two agents request refunds on the same payment at the same moment. Walk me through what your code does.
3. How do you identify the user and their role? Could an agent call the approve endpoint directly with Postman?
4. A supervisor rejects a refund. Does the available refund amount increase again? Show me.
5. Where would you add an audit log, and what would each entry contain?

## Live change

Block a supervisor from approving a refund on a payment they requested themselves.

## Their README questions (ask them to explain)

1. Two agents request a refund on the same payment at the same moment. How do you stop the total going over the limit?
2. What would you add before this went to production?

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
