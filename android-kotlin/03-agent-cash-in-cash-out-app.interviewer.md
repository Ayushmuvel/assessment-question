# AND-03: Agent Cash-In / Cash-Out — Interviewer notes

> **Private. Do not share with candidates or push to GitHub.**
> Assessment sent to the candidate: [03-agent-cash-in-cash-out-app.md](03-agent-cash-in-cash-out-app.md)

## Call plan (30–40 min)

1. **Demo (5 min):** the candidate shares their screen and runs the project.
2. **Cross-questions (15 min):** the questions below. Ask them to open the exact file or line.
3. **Live change (10 min):** see below. AI tools are allowed, but they must explain what they did.
4. **README answers (5 min):** ask them to explain their written answers in their own words.

## Cross-questions

1. Show me the limit checks. Where do they run: ViewModel, repository, or UI? Why there?
2. How does the inactivity timeout work? What resets it? What happens if the app is in the background for 5 minutes?
3. Do the dashboard totals come from a single source of truth, or are they changed by hand after each transaction?
4. Search your code for `Log.` calls. Does any log print an OTP, PIN, or token?
5. What happens if the agent taps "Confirm" twice on cash-out?

## Live change

Add a daily cash-out limit of ₹20,000 per customer.

## Their README questions (ask them to explain)

1. How did you implement the inactivity timeout, and how does it behave when the app goes to the background?
2. Which data must never appear in logs in an app like this?

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
