# FE-01: Merchant Transactions Dashboard — Interviewer notes

> **Private. Do not share with candidates or push to GitHub.**
> Assessment sent to the candidate: [01-merchant-transactions-dashboard.md](01-merchant-transactions-dashboard.md)

## Call plan (30–40 min)

1. **Demo (5 min):** the candidate shares their screen and runs the project.
2. **Cross-questions (15 min):** the questions below. Ask them to open the exact file or line.
3. **Live change (10 min):** see below. AI tools are allowed, but they must explain what they did.
4. **README answers (5 min):** ask them to explain their written answers in their own words.

## Cross-questions

1. Point to each component: is it a Server or Client Component? Why did you put `'use client'` where you did?
2. Show me how the filters get into the URL and back out on refresh.
3. Type quickly in the search box. How many requests are sent? How would you reduce them?
4. Where is the filtering done: in the browser or in the API route? What changes with 1 million rows?
5. Show me the error state. How did you test it?

## Live change

Add a "method" filter (UPI / Card / Wallet) that is also kept in the URL.

## Their README questions (ask them to explain)

1. Which parts are Server Components and which are Client Components, and why?
2. What would you change if there were 1 million transactions instead of 200?

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
