# FL-02: Scan & Pay (Merchant QR)

**Role:** Flutter Mobile App Developer  
**Stack:** Flutter and Dart, with Bloc, Riverpod, Provider or a similar state management approach.

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **FL-02** in your repository README and in your reply email.

## Context
Customers pay at shops by scanning the merchant's QR code. The flow must be fast and clear, even when the camera permission is denied or the QR is invalid.

## Requirements

- **Scan screen** using the camera (`mobile_scanner` or similar).
  - Handle camera permission: granted, denied, and permanently denied (with a link to settings).
  - A "Pick from gallery" option as a fallback.
- Parse a QR payload in this format (create a few test QR images and include them in the repo):
  `upi://pay?pa=merchant@bank&pn=Chai%20Point&am=120.00&tn=Order123`
  - If `am` is present, the amount is fixed. If not, the user enters it.
  - Show a clear error for invalid or unsupported QR codes.
- **Confirm screen:** merchant name, UPI ID, amount, note. Confirm with PIN or **device biometrics** (`local_auth`).
- **Processing → Success / Failed** screens. Success shows a receipt that can be **shared as an image or PDF**.
- **History:** list of past QR payments, stored locally.
- Deep link support: opening `jstackpay://pay?...` launches the confirm screen directly.

## Bonus
- Integration test for the scan → confirm → success flow (with a mocked scanner).
- Haptic feedback and sound on success.
