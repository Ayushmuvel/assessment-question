# AND-01: Merchant Collect (QR) App

**Role:** Android Developer (Kotlin) · **Time:** about 3 hours (bonus is optional)

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **AND-01** in your repository README and in your reply email.

## Scenario

Shopkeepers use our app to collect payments: they enter an amount, show a QR code, and wait for the payment to be confirmed.

## Must have

Mock the API inside the app (a fake repository with delays). No backend is needed.

- Kotlin, Jetpack Compose, MVVM, Coroutines and Flow.
- **Collect screen:** enter an amount and generate a QR code for
  `upi://pay?pa=shop@bank&pn=Shop&am=250.00&tr=<txnRef>`
- **Waiting state:** check the payment status every 3 seconds until `SUCCESS`, `FAILED` or a 2-minute timeout.
  - Checking stops when the user leaves the screen.
  - Rotating the screen does not restart the payment or lose state.
- **Result screen** for success, failure and timeout.
- Submit a 2–3 minute screen recording (an APK is welcome).

## Bonus (optional)

- Today's collections list, cached in Room.
- Hilt for dependency injection.
- Block screenshots on payment screens (`FLAG_SECURE`).
- A ViewModel unit test.

## Answer in your README (a few lines each)

1. How did you make sure the status checks stop and do not leak when the screen closes?
2. What happens to the waiting state after process death, and how would you handle it?

If you run out of time, list what you skipped and how you would build it.
