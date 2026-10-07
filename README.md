# jStack Engineering Assessments

Welcome, and thank you for your interest in joining jStack.

jStack builds financial infrastructure for banks, fintechs, telecom operators and businesses: digital wallets, mobile money, payment gateways, payment orchestration and transaction management platforms. The assessments in this repository are small versions of problems we solve every day.

## Find your assessment

Your invitation email gives you an **assessment ID** (for example, `FS-02`). Open the matching file below and complete **only that assessment**.

| Role | ID | Assessment |
| --- | --- | --- |
| [Full Stack Engineer (MERN)](full-stack-mern/README.md) | FS-01 | [P2P Wallet Transfer](full-stack-mern/01-p2p-wallet-transfer.md) |
| | FS-02 | [Merchant Payment Links](full-stack-mern/02-merchant-payment-links.md) |
| | FS-03 | [Refund Portal](full-stack-mern/03-refund-and-dispute-portal.md) |
| | FS-04 | [Wallet Top-Up via Payment Gateway](full-stack-mern/04-wallet-top-up-via-payment-gateway.md) |
| | FS-05 | [Agent Commission Ledger](full-stack-mern/05-agent-commission-ledger.md) |
| [Backend Engineer (Node.js)](backend-nodejs/README.md) | BE-01 | [Payment Status Service](backend-nodejs/01-payment-orchestration-service.md) |
| | BE-02 | [Double-Entry Ledger](backend-nodejs/02-double-entry-ledger-and-reconciliation.md) |
| | BE-03 | [Transaction Risk Checks](backend-nodejs/03-transaction-risk-and-velocity-checks.md) |
| [Frontend Engineer (React / Next.js)](frontend-react-nextjs/README.md) | FE-01 | [Merchant Transactions Dashboard](frontend-react-nextjs/01-merchant-transactions-dashboard.md) |
| | FE-02 | [Checkout Page](frontend-react-nextjs/02-checkout-page.md) |
| | FE-03 | [KYC Onboarding Wizard](frontend-react-nextjs/03-kyc-onboarding-wizard.md) |
| [.NET Developer](dotnet-developer/README.md) | NET-01 | [Wallet API](dotnet-developer/01-wallet-api.md) |
| | NET-02 | [Settlement Reconciliation](dotnet-developer/02-settlement-and-reconciliation-job.md) |
| | NET-03 | [Payout Approval API (Maker-Checker)](dotnet-developer/03-payout-api-with-maker-checker-approval.md) |
| [QA Engineer / Software Tester](qa-engineer/README.md) | QA-01 | [Test Design for a Wallet Transfer](qa-engineer/01-test-design-for-a-wallet-transfer.md) |
| | QA-02 | [API Test Suite](qa-engineer/02-api-test-suite.md) |
| | QA-03 | [UI Automation for a Checkout Flow](qa-engineer/03-ui-automation-for-a-checkout-flow.md) |
| [Flutter Mobile App Developer](flutter-developer/README.md) | FL-01 | [Wallet App – Send Money](flutter-developer/01-wallet-app-send-money.md) |
| | FL-02 | [Scan & Pay (Merchant QR)](flutter-developer/02-scan-and-pay-merchant-qr.md) |
| | FL-03 | [Offline-First Transaction History](flutter-developer/03-offline-first-transaction-history-and-insights.md) |
| [Android Developer (Kotlin)](android-kotlin/README.md) | AND-01 | [Merchant Collect (QR) App](android-kotlin/01-merchant-collect-qr-app.md) |
| | AND-02 | [Wallet with Reliable Transfers](android-kotlin/02-wallet-with-reliable-transfers.md) |
| | AND-03 | [Agent Cash-In / Cash-Out](android-kotlin/03-agent-cash-in-cash-out-app.md) |

## How it works

1. Open the assessment matching the ID in your invitation email.
2. Build the **Must have** part. Each assessment is designed for **about 3 hours**. Additional tasks are optional and earn plus points. If you run out of time, list what you skipped in your README and how you would build it.
3. Push your work to a **public GitHub repository** (or a private one shared with the email given in your invitation).
4. Reply to your invitation email with the repository link and your assessment ID **before the deadline** stated there.
5. In the next round, you will walk us through your solution and answer questions about your choices.

## What your submission must include

- Source code with a clear folder structure.
- A `README.md` with:
  - your assessment ID
  - how to install and run the project (and tests, if any)
  - any assumptions you made
  - your answers to the short questions at the end of the assessment
  - what you would improve with more time
- For mobile apps: an APK or a short screen recording (2–3 minutes) of the app running.
- For QA assessments: the documents and test files listed in the task.

## Rules

- You may use any language features, libraries, documentation and AI tools. **You must be able to explain every line you submit.** The follow-up interview goes through your code in detail.
- Mock APIs, mock data and in-memory databases are fine unless the task says otherwise.
- Do not include real card numbers, real personal data or real API keys. Use test data.
- Keep commits meaningful. We like to see how your solution evolved.

## What we look for

| Area | What it means |
| --- | --- |
| Correctness | It works and meets the stated requirements |
| Money safety | Money is never lost, created or moved twice, even with retries, double clicks or timeouts |
| Failure handling | Clear behaviour when the network, a provider or the user does something unexpected |
| Code quality | Readable, well-structured, sensible names, no dead code |
| Testing | Meaningful tests for the important logic |
| Communication | A README that lets us run your project in 5 minutes and understand your decisions |

We value a smaller, solid solution over a larger, fragile one.

## Questions

If something is unclear, make a reasonable assumption and write it in your README. You can also reply to your invitation email.

Good luck!
