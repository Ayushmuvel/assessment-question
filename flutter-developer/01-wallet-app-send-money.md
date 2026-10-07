# FL-01: Wallet App – Send Money

**Role:** Flutter Mobile App Developer  
**Stack:** Flutter and Dart, with Bloc, Riverpod, Provider or a similar state management approach.

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **FL-01** in your repository README and in your reply email.

## Context
Users in areas with weak networks use our wallet app to send money. Tapping "Send" twice or losing signal halfway must never send money twice.

## Requirements

- **Login** with mobile number + mock OTP (`123456`). Store the session token in **secure storage** (`flutter_secure_storage`), not plain shared preferences.
- **Home:** wallet balance and the last 10 transactions, with pull-to-refresh.
- **Send Money:**
  - pick a contact from a mock list or enter a mobile number
  - enter an amount (₹1 – ₹50,000) with validation
  - confirm with a 4-digit PIN screen
  - the button is disabled and shows a loader while the request is pending
  - each transfer carries a unique request ID; on timeout, retry **once with the same request ID**
  - result screens: Success, Failed (with reason), and "Processing" for timeouts
- **Offline:** show an offline banner and the last cached balance and transactions (Hive, sqflite or Isar).
- Clean structure: UI, state, repository and API layers separated.

## Bonus
- Unit tests for the state logic and one widget test for the Send Money screen.
- Dark mode.
- App lock after 2 minutes in the background.
