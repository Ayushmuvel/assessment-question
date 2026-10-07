# FE-02: Checkout Page — Interviewer notes

> **Private. Do not share with candidates or push to GitHub.**
> Assessment sent to the candidate: [02-checkout-page.md](02-checkout-page.md)

## Call plan (30–40 min)

1. **Demo (5 min):** the candidate shares their screen and runs the project.
2. **Cross-questions (15 min):** the questions below. Ask them to open the exact file or line.
3. **Live change (10 min):** see below. AI tools are allowed, but they must explain what they did.
4. **README answers (5 min):** ask them to explain their written answers in their own words.

## Cross-questions

1. Show me the Luhn check. Explain how it works for a short card number.
2. Where is the idempotency key created? Does it change if the user retries after `FAILED`? Should it?
3. Double-click Pay with the network throttled to Slow 3G in DevTools. Show me that only one request goes out.
4. On `TIMEOUT`, why must the page not offer to pay again?
5. Use only the keyboard to complete a payment. Where does focus go after an error?

## Live change

Show a 10-minute countdown that blocks payment when it reaches zero.

## Their README questions (ask them to explain)

1. Why is disabling the button not enough on its own to prevent double payments?
2. What should the page show if the user closes the tab during processing and comes back?

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
