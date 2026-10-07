# FL-01: Wallet App – Send Money — Interviewer notes

> **Private. Do not share with candidates or push to GitHub.**
> Assessment sent to the candidate: [01-wallet-app-send-money.md](01-wallet-app-send-money.md)

## Call plan (30–40 min)

1. **Demo (5 min):** the candidate shares their screen and runs the project.
2. **Cross-questions (15 min):** the questions below. Ask them to open the exact file or line.
3. **Live change (10 min):** see below. AI tools are allowed, but they must explain what they did.
4. **README answers (5 min):** ask them to explain their written answers in their own words.

## Cross-questions

1. Show me where the request ID is created. When the retry happens, show me that it reuses the same ID.
2. Walk me through the states in your Bloc / Riverpod / Provider for Send Money. Which state disables the button?
3. Tap Send twice very fast in the running app. Why does only one request go out?
4. How does the Home balance update after a transfer? Is it refetched or updated locally? What are the risks of each?
5. Where is the mock repository? How would you swap it for a real API without touching the UI?

## Live change

Add a confirmation dialog showing the amount and receiver before sending.

## Their README questions (ask them to explain)

1. Why reuse the same request ID on retry? What would go wrong with a new one?
2. The user kills the app while a transfer is "Processing". What should they see when they open it again?

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
