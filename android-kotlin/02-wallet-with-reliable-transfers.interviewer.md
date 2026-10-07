# AND-02: Wallet with Reliable Transfers — Interviewer notes

> **Private. Do not share with candidates or push to GitHub.**
> Assessment sent to the candidate: [02-wallet-with-reliable-transfers.md](02-wallet-with-reliable-transfers.md)

## Call plan (30–40 min)

1. **Demo (5 min):** the candidate shares their screen and runs the project.
2. **Cross-questions (15 min):** the questions below. Ask them to open the exact file or line.
3. **Live change (10 min):** see below. AI tools are allowed, but they must explain what they did.
4. **README answers (5 min):** ask them to explain their written answers in their own words.

## Cross-questions

1. Show me the Room write that happens before the API call. Why that order?
2. Open the WorkManager code. Which constraints and backoff did you use? Is the work unique per transfer?
3. Kill the app while a transfer is `SENDING`. What happens next time WorkManager runs?
4. Show me where the same `requestId` is reused on retry.
5. How does the transfers list update when the worker finishes?

## Live change

After 5 failed attempts, mark the transfer `FAILED` and stop retrying.

## Their README questions (ask them to explain)

1. The app is killed while a transfer is `SENDING`. What happens when WorkManager runs again?
2. Why save the transfer in Room before calling the API?

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
