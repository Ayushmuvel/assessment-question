# QA-03: UI Automation for a Checkout Flow — Interviewer notes

> **Private. Do not share with candidates or push to GitHub.**
> Assessment sent to the candidate: [03-ui-automation-for-a-checkout-flow.md](03-ui-automation-for-a-checkout-flow.md)

## Call plan (30–40 min)

1. **Walkthrough (5 min):** the candidate shares their screen and walks through what they submitted.
2. **Cross-questions (15 min):** the questions below. Ask them to open the exact file or line.
3. **Live exercise (10 min):** see below. AI tools are allowed, but they must explain what they did.
4. **README answers (5 min):** ask them to explain their written answers in their own words.

## Cross-questions

1. Open one page object and explain how it is organised. Why put locators there and not in the test?
2. Show me how you wait for elements. Did you use any fixed `sleep` or `wait`? Why is that a problem?
3. Run the tests now. If one fails, how do you decide whether it's a bug or a flaky test?
4. Show me how the total is verified. What if tax is rounded differently?
5. Show me one `problem_user` bug you found and how you would automate a check for it.

## Live exercise

Add a test that removes an item from the cart during checkout and checks the new total.

## Their README questions (ask them to explain)

1. How do you keep UI tests from being flaky?
2. Which tests here would you run on every commit, and which only before a release?

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
| Live exercise |  |  |
| Failure / fintech thinking |  |  |
| Communication |  |  |

Candidate: ______ · Interviewer: ______ · Date: ______ · Recommendation: Hire / Second opinion / No hire
