# FL-01: Wallet App – Send Money

**Role:** Flutter Mobile App Developer · **Time:** about 3 hours (additional tasks are optional)

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **FL-01** in your repository README and in your reply email.

## Scenario

Users on weak networks send money with our app. Tapping "Send" twice or losing signal must never send money twice.

## Must have

Mock everything inside the app (a fake repository with delays and random failures). No backend is needed.

- **Home screen:** balance and recent transactions.
- **Send Money screen:** receiver mobile number and amount (₹1 – ₹50,000) with validation.
  - The button is disabled and shows a loader while the request is pending.
  - Each transfer has a unique request ID. On a timeout, retry **once with the same request ID**.
  - Result: Success, Failed (with a reason), or "Processing" after a second timeout.
  - The balance on Home updates after a successful transfer.
- State management with Bloc, Riverpod or Provider. Keep UI, state and data layers separate.
- Submit a 2–3 minute screen recording (an APK is welcome).

## Additional tasks (optional, plus points)

Not required. Each one you complete counts in your favour.

- Use an HTTP client with a mock adapter (for example Dio, or `http` with `MockClient`) and parse JSON responses into model classes.
- PIN confirmation screen before sending.
- Store a mock session token in `flutter_secure_storage`.
- Cache the last balance and transactions for offline viewing.
- Unit tests for the state logic.

## Answer in your README (a few lines each)

1. Why reuse the same request ID on retry? What would go wrong with a new one?
2. The user kills the app while a transfer is "Processing". What should they see when they open it again?

If you run out of time, list what you skipped and how you would build it.
