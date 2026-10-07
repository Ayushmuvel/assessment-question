# AND-02: Wallet with Reliable Transfers

**Role:** Android Developer (Kotlin) · **Time:** about 3 hours (bonus is optional)

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **AND-02** in your repository README and in your reply email.

## Scenario

Our users often lose network while sending money. A transfer started offline must be sent later, exactly once.

## Must have

Mock the API inside the app (a fake repository that can fail or time out). No backend is needed.

- Kotlin, Jetpack Compose, MVVM, Coroutines and Flow, Room.
- **Send Money screen:** receiver mobile number and amount (₹1 – ₹50,000) with validation.
- Every transfer gets a unique `requestId` and is **saved in Room before** calling the API.
- If offline or the call fails, the transfer stays `QUEUED` and is sent later by **WorkManager** (with a network constraint).
- Retries always reuse the same `requestId`.
- **Transfers list** showing each transfer's state: `QUEUED`, `SENDING`, `SUCCESS` or `FAILED`.
- Submit a 2–3 minute screen recording (an APK is welcome).

## Bonus (optional)

- Hilt for dependency injection.
- Biometric unlock (`BiometricPrompt`).
- Unit tests for the queue and retry logic.

## Answer in your README (a few lines each)

1. The app is killed while a transfer is `SENDING`. What happens when WorkManager runs again?
2. Why save the transfer in Room before calling the API?

If you run out of time, list what you skipped and how you would build it.
