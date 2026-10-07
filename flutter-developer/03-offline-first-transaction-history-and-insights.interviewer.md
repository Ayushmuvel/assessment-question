# FL-03: Offline-First Transaction History — Interviewer notes

> **Private. Do not share with candidates or push to GitHub.**
> Assessment sent to the candidate: [03-offline-first-transaction-history-and-insights.md](03-offline-first-transaction-history-and-insights.md)

## Call plan (30–40 min)

1. **Demo (5 min):** the candidate shares their screen and runs the project.
2. **Cross-questions (15 min):** the questions below. Ask them to open the exact file or line.
3. **Live change (10 min):** see below. AI tools are allowed, but they must explain what they did.
4. **README answers (5 min):** ask them to explain their written answers in their own words.

## Cross-questions

1. Turn on airplane mode and restart the app. Show me where cached data is loaded.
2. How do you know when to load the next page? What stops the same page loading twice?
3. Search while offline. Does it search the cache or fail? Why?
4. Show me how the cache is updated after a refresh. What happens to a transaction that changed status?
5. In DevTools, how would you prove the list doesn't rebuild every item when one changes?

## Live change

Add a status filter (Success / Failed / Pending) that also works offline.

## Their README questions (ask them to explain)

1. How do you decide when cached data is too old to show?
2. How did you keep scrolling smooth? How would you check for performance problems?

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
