# AND-02: Wallet with Reliable Transfers

**Role:** Android Developer (Kotlin)  
**Stack:** Kotlin, Jetpack Compose (XML is acceptable where it makes sense), MVVM, Coroutines and Flow, Retrofit/OkHttp, Room, Hilt.

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **AND-02** in your repository README and in your reply email.

## Context
Our users often lose network while sending money. A transfer started offline must be sent later, exactly once.

## Requirements

- **Home:** balance and recent transactions (Room cache + network refresh with Flow).
- **Send Money:** receiver mobile number, amount (₹1 – ₹50,000), 4-digit PIN confirmation.
  - Every transfer gets a unique `requestId`, saved locally **before** calling the API.
  - If offline, save the transfer as `QUEUED` and send it later with **WorkManager** (network constraint, exponential backoff).
  - Retries always reuse the same `requestId`, so the server can ignore duplicates.
  - The UI clearly shows `QUEUED`, `SENDING`, `SUCCESS` and `FAILED` states for each transfer.
- **Biometric unlock** (`BiometricPrompt`) when opening the app, with PIN as a fallback.
- Handle errors clearly: no network, server error, insufficient balance, session expired (go to login).
- Dependency injection with Hilt; coroutines with correct scopes and dispatchers.

## Bonus
- Unit tests for the queue/retry logic and an instrumentation or Compose UI test for the Send Money screen.
- Firebase Cloud Messaging (or a local notification) when a queued transfer completes.
