# FS-04: Wallet Top-Up via Payment Gateway — Interviewer notes

> **Private. Do not share with candidates or push to GitHub.**
> Assessment sent to the candidate: [04-wallet-top-up-via-payment-gateway.md](04-wallet-top-up-via-payment-gateway.md)

## Call plan (30–40 min)

1. **Demo (5 min):** the candidate shares their screen and runs the project.
2. **Cross-questions (15 min):** the questions below. Ask them to open the exact file or line.
3. **Live change (10 min):** see below. AI tools are allowed, but they must explain what they did.
4. **README answers (5 min):** ask them to explain their written answers in their own words.

## Cross-questions

1. Show me where the wallet is credited. Which event triggers it: the redirect, the webhook, or the status check? Why?
2. The webhook arrives twice. Show me the line that stops a second credit.
3. The user comes back before the webhook arrives. What do they see, and what if the webhook never arrives?
4. Are the order status update and the wallet credit atomic? What if the server crashes between them?
5. If you used Socket.io: how does the server know which socket belongs to which user?

## Live change

Add a maximum of 3 top-ups per user per day.

## Their README questions (ask them to explain)

1. The user comes back from the gateway before the webhook arrives. What does the user see, and why is it safe?
2. Why should you never credit the wallet based on the browser redirect alone?

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
