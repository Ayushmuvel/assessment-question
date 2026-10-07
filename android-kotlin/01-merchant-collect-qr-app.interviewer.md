# AND-01: Merchant Collect (QR) App — Interviewer notes

> **Private. Do not share with candidates or push to GitHub.**
> Assessment sent to the candidate: [01-merchant-collect-qr-app.md](01-merchant-collect-qr-app.md)

## Call plan (30–40 min)

1. **Demo (5 min):** the candidate shares their screen and runs the project.
2. **Cross-questions (15 min):** the questions below. Ask them to open the exact file or line.
3. **Live change (10 min):** see below. AI tools are allowed, but they must explain what they did.
4. **README answers (5 min):** ask them to explain their written answers in their own words.

## Cross-questions

1. Show me where status checking starts and stops. Which coroutine scope runs it, and why does it stop when the screen closes?
2. Rotate the device during waiting. Show me why the payment isn't restarted.
3. Is your UI state a `StateFlow`, `LiveData`, or Compose state? Why that choice?
4. How is `txnRef` generated? Could two payments get the same one?
5. Kill the app from recent apps while waiting, then reopen it. What happens, and what should happen?

## Live change

Add a "Cancel" button that stops waiting and marks the payment as cancelled.

## Their README questions (ask them to explain)

1. How did you make sure the status checks stop and do not leak when the screen closes?
2. What happens to the waiting state after process death, and how would you handle it?

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
