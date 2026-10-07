# FS-05: Agent Commission Ledger — Interviewer notes

> **Private. Do not share with candidates or push to GitHub.**
> Assessment sent to the candidate: [05-agent-commission-ledger.md](05-agent-commission-ledger.md)

## Call plan (30–40 min)

1. **Demo (5 min):** the candidate shares their screen and runs the project.
2. **Cross-questions (15 min):** the questions below. Ask them to open the exact file or line.
3. **Live change (10 min):** see below. AI tools are allowed, but they must explain what they did.
4. **README answers (5 min):** ask them to explain their written answers in their own words.

## Cross-questions

1. Calculate the split for a ₹333 cash-in out loud, then show me the code that does it. Where does the leftover paisa go?
2. Why integer paise? Show me what would go wrong with `0.1 + 0.2` in JavaScript.
3. Show me how a distributor sees only their own agents. What happens if an agent changes the ID in the URL?
4. How is the summary calculated: in JavaScript or a MongoDB aggregation? What happens with 1 million cash-ins?
5. The commission rate changes next month. Do old commissions change? Show me why not.

## Live change

Change the split to 60/40 for one specific distributor only.

## Their README questions (ask them to explain)

1. A cash-in of ₹333 gives a commission of ₹3.33. How exactly is it split, and where does the leftover paisa go?
2. How did you make sure an agent can never see another agent's data, even by changing an ID in the URL?

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
