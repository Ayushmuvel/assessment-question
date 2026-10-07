# FE-03: KYC Onboarding Wizard — Interviewer notes

> **Private. Do not share with candidates or push to GitHub.**
> Assessment sent to the candidate: [03-kyc-onboarding-wizard.md](03-kyc-onboarding-wizard.md)

## Call plan (30–40 min)

1. **Demo (5 min):** the candidate shares their screen and runs the project.
2. **Cross-questions (15 min):** the questions below. Ask them to open the exact file or line.
3. **Live change (10 min):** see below. AI tools are allowed, but they must explain what they did.
4. **README answers (5 min):** ask them to explain their written answers in their own words.

## Cross-questions

1. Show me where progress is saved. What personal data ends up in the browser, and is that a risk?
2. Show me the 18+ check. What happens for a user whose 18th birthday is today?
3. How is each step validated: one schema per step or one big schema? Why?
4. Go back from step 3 to step 1 and change the mobile number. What should happen to the OTP step?
5. Inspect the markup. How is each error message linked to its input for screen readers?

## Live change

Add an "Occupation" dropdown to step 2 that is required only for users over 60.

## Their README questions (ask them to explain)

1. Where did you save progress, and what are the security risks of storing personal data there?
2. How would you make this wizard accessible to screen-reader users?

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
