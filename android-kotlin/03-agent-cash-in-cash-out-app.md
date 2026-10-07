# AND-03: Agent Cash-In / Cash-Out

**Role:** Android Developer (Kotlin) · **Time:** about 3 hours (bonus is optional)

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **AND-03** in your repository README and in your reply email.

## Scenario

In mobile money, local agents help customers deposit (cash-in) and withdraw (cash-out) money. Agents serve many customers a day, so the app must be quick and safe.

## Must have

Mock the API inside the app (a fake repository with mock balances). No backend is needed.

- Kotlin, Jetpack Compose, MVVM, Coroutines and Flow.
- **Dashboard:** the agent's float balance and today's cash-in and cash-out totals.
- **Cash-In flow:** customer mobile → amount → confirm → receipt screen.
  - cash-in cannot exceed the agent's float
- **Cash-Out flow:** customer mobile → amount → customer OTP (mock `123456`) → receipt screen.
  - cash-out cannot exceed the customer's balance
- The dashboard totals update after each transaction.
- **Session timeout:** after 3 minutes of inactivity, return to a PIN screen.
- Submit a 2–3 minute screen recording (an APK is welcome).

## Bonus (optional)

- Scan the customer's QR code to fill the mobile number.
- Transaction history cached in Room.
- Share the receipt as an image.
- Unit tests for the limit rules.

## Answer in your README (a few lines each)

1. How did you implement the inactivity timeout, and how does it behave when the app goes to the background?
2. Which data must never appear in logs in an app like this?

If you run out of time, list what you skipped and how you would build it.
