# FS-01: P2P Wallet Transfer — Interviewer notes

> **Private. Do not share with candidates or push to GitHub.**
> Assessment sent to the candidate: [01-p2p-wallet-transfer.md](01-p2p-wallet-transfer.md)

## Call plan (30–40 min)

1. **Demo (5 min):** the candidate shares their screen and runs the project.
2. **Cross-questions (15 min):** the questions below. Ask them to open the exact file or line.
3. **Live change (10 min):** see below. AI tools are allowed, but they must explain what they did.
4. **README answers (5 min):** ask them to explain their written answers in their own words.

## Cross-questions

1. Show me where the `Idempotency-Key` is checked. Where is it stored, and what happens if two requests with the same key arrive at the exact same millisecond?
2. What does the second call with the same key return: the same response, an error, or something else? Why?
3. Show me the balance check. Is it a read followed by a write? What happens with two parallel transfers of ₹8,000 from a ₹10,000 wallet?
4. If you used a MongoDB transaction: what does it need to work (replica set)? If not: what can go wrong between the debit and the credit?
5. The same key arrives with a different amount. What does your code do? What should it do?

## Live change

Add a daily limit of ₹25,000 per sender.

## Their README questions (ask them to explain)

1. What happens if the server crashes after debiting the sender but before crediting the receiver?
2. What happens if two transfers from the same wallet run at the same moment?

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
