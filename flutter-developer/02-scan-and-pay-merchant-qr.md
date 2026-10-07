# FL-02: Scan & Pay (Merchant QR)

**Role:** Flutter Mobile App Developer · **Time:** about 3 hours (bonus is optional)

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **FL-02** in your repository README and in your reply email.

## Scenario

Customers pay at shops by scanning the merchant's QR code. The flow must be quick and clear, even when the camera permission is denied or the QR code is invalid.

## Must have

- **Scan screen** using the camera (`mobile_scanner` or similar).
  - Handle camera permission granted, denied and permanently denied (with a button to open settings).
- Parse a QR payload in this format:
  `upi://pay?pa=merchant@bank&pn=Chai%20Point&am=120.00`
  - if `am` is present, the amount is fixed; if not, the user enters it
  - show a clear error for an invalid QR code
- **Confirm screen:** merchant name, UPI ID and amount, with a "Pay" button.
- **Result screen:** a mock payment returns success or failure.
- Include 2–3 test QR images in your repository.
- Submit a 2–3 minute screen recording (an APK is welcome).

## Bonus (optional)

- Confirm with device biometrics (`local_auth`).
- "Pick QR from gallery" option.
- Local history of past payments.

## Answer in your README (a few lines each)

1. What can go wrong when trusting data from a scanned QR code, and how do you protect the user?
2. How would you test the scanning flow without a real camera?

If you run out of time, list what you skipped and how you would build it.
