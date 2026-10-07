# AND-03: Agent Cash-In / Cash-Out App

**Role:** Android Developer (Kotlin)  
**Stack:** Kotlin, Jetpack Compose (XML is acceptable where it makes sense), MVVM, Coroutines and Flow, Retrofit/OkHttp, Room, Hilt.

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **AND-03** in your repository README and in your reply email.

## Context
In mobile money, local agents help customers deposit (cash-in) and withdraw (cash-out) money. Agents work in shops with weak networks and handle many customers a day, so the app must be quick and safe.

## Requirements

- **Agent login** with agent code + PIN, then biometric unlock on later launches.
- **Dashboard:** agent float balance, today's cash-in and cash-out totals, number of transactions.
- **Cash-In flow:** customer mobile number → amount → customer confirmation via mock OTP (`123456`) → agent PIN → receipt.
- **Cash-Out flow:** customer mobile number → amount → customer approves with mock OTP → agent PIN → receipt.
  - Cash-out cannot exceed the customer's mock balance; cash-in cannot exceed the agent's float.
- **Scan customer QR** (CameraX + ML Kit or ZXing) as a faster way to fill the mobile number.
- **Receipts:** share as an image or PDF.
- **Transaction history** with filters (type, date, status), cached in Room.
- **Session timeout:** after 3 minutes of inactivity, require the PIN again.
- No sensitive data (PIN, OTP, tokens) in logs.

## Bonus
- Print the receipt to a Bluetooth printer (or a stub with a clear interface for it).
- Unit tests for the limit rules and the session timeout.
