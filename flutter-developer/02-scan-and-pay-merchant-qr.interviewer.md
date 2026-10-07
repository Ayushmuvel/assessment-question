# FL-02: Scan & Pay (Merchant QR) — Interviewer notes

> **Private. Do not share with candidates or push to GitHub.**
> Assessment sent to the candidate: [02-scan-and-pay-merchant-qr.md](02-scan-and-pay-merchant-qr.md)

## Call plan (30–40 min)

1. **Demo (5 min):** the candidate shares their screen and runs the project.
2. **Cross-questions (15 min):** the questions below. Ask them to open the exact file or line.
3. **Live change (10 min):** see below. AI tools are allowed, but they must explain what they did.
4. **README answers (5 min):** ask them to explain their written answers in their own words.

## Cross-questions

1. Show me the QR parsing code. What happens with `am=abc`, a negative amount, or a missing `pa`?
2. Deny camera permission permanently on the device. Show me what the user sees.
3. A QR code says the merchant name is "Bank Refund Department". What risks does trusting QR data create?
4. How does the scanner avoid scanning the same QR code five times in one second?
5. How would you test the scan flow without a camera?

## Live change

Reject payments above ₹2,000 for QR codes with no fixed amount.

## Their README questions (ask them to explain)

1. What can go wrong when trusting data from a scanned QR code, and how do you protect the user?
2. How would you test the scanning flow without a real camera?

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
