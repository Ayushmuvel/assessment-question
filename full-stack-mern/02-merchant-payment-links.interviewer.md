# FS-02: Merchant Payment Links — Interviewer notes

> **Private. Do not share with candidates or push to GitHub.**
> Assessment sent to the candidate: [02-merchant-payment-links.md](02-merchant-payment-links.md)

## Call plan (30–40 min)

1. **Demo (5 min):** the candidate shares their screen and runs the project.
2. **Cross-questions (15 min):** the questions below. Ask them to open the exact file or line.
3. **Live change (10 min):** see below. AI tools are allowed, but they must explain what they did.
4. **README answers (5 min):** ask them to explain their written answers in their own words.

## Cross-questions

1. Show me where a duplicate `eventId` is ignored. Is it stored before or after the payment is updated? Why does the order matter?
2. Walk me through `FAILED` arriving after `SUCCESS`. Which line stops the status changing?
3. Two customers press "Pay" on the same link at the same moment. Show me what prevents two payments.
4. How does the merchant page know a link has been paid?
5. Anyone can call your webhook URL. What stops a fake `SUCCESS`?

## Live change

Add link expiry, so an expired link returns a clear error on Pay.

## Their README questions (ask them to explain)

1. The provider sends `FAILED` after `SUCCESS` for the same payment. What does your system do, and why?
2. How would you make sure a link is never paid twice if two customers press "Pay" at the same moment?

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
