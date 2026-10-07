# AND-01: Merchant Collect (QR) App

**Role:** Android Developer (Kotlin)  
**Stack:** Kotlin, Jetpack Compose (XML is acceptable where it makes sense), MVVM, Coroutines and Flow, Retrofit/OkHttp, Room, Hilt.

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **AND-01** in your repository README and in your reply email.

## Context
Shopkeepers use our app to collect payments. They enter an amount, show a QR code to the customer, and wait for the payment to be confirmed.

## Requirements

- **Login** with merchant ID and PIN. Store the token with **EncryptedSharedPreferences** or **DataStore + Android Keystore**.
- **Collect screen:**
  - enter an amount and an optional note
  - generate a QR code for a payload like `upi://pay?pa=shop@bank&pn=Shop&am=250.00&tr=<txnRef>`
  - observe the payment status (polling every 3 seconds is fine) until `SUCCESS`, `FAILED` or a 5-minute timeout
  - polling stops when the screen closes, and resumes correctly after rotation
- **Success screen** with a sound and a short summary.
- **Today's collections:** list from the API, cached in **Room** as the single source of truth; works offline.
- Rotation and process death must not lose state or repeat API calls (`ViewModel` + `SavedStateHandle`).
- Block screenshots on payment screens (`FLAG_SECURE`).

## Bonus
- Unit tests for the ViewModel with a fake repository.
- A daily summary notification using WorkManager.
