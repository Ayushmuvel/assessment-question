# NET-02: Settlement Reconciliation — Interviewer notes

> **Private. Do not share with candidates or push to GitHub.**
> Assessment sent to the candidate: [02-settlement-and-reconciliation-job.md](02-settlement-and-reconciliation-job.md)

## Call plan (30–40 min)

1. **Demo (5 min):** the candidate shares their screen and runs the project.
2. **Cross-questions (15 min):** the questions below. Ask them to open the exact file or line.
3. **Live change (10 min):** see below. AI tools are allowed, but they must explain what they did.
4. **README answers (5 min):** ask them to explain their written answers in their own words.

## Cross-questions

1. Show me the matching logic. What is its complexity with 1 million rows on each side?
2. How do you detect that the same file was uploaded twice: file name, hash, or content? What are the weaknesses of each?
3. A row has an amount of `100.10` in the file and `100.1` in the database. Does your code match them? Show me.
4. The CSV has a broken row in the middle. What does your code do with the rest of the file?
5. Where would you move processing into a background service, and how would the user see progress?

## Live change

Add a fifth group: duplicate rows inside the bank file.

## Their README questions (ask them to explain)

1. How would you handle a file with 1 million rows?
2. How did you detect that the same file was uploaded twice?

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
