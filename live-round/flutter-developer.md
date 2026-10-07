# Flutter Developer: Live Round Tasks

Paste only the **Prompt** and **Starter code** into the call chat. Answer keys are for the interviewer.
DartPad (dartpad.dev) works for FL-L1 and FL-L3 if the candidate's Flutter setup is slow.

---

## FL-L1: Parse a UPI QR string

**Time:** 15 min · **Type:** coding (Dart)

**Prompt**

> Write `UpiPayment? parseUpi(String raw)` for strings like
> `upi://pay?pa=shop@okbank&pn=Chai%20Point&am=120.50&tn=Order12`
> - `pa` (UPI ID) is required and must look like `name@bank`.
> - `pn` (name) is optional; decode `%20` etc.
> - `am` is optional; if present it must be a positive number with at most 2 decimals. Store it in **paise**.
> - Return `null` for anything invalid.

**Answer key**

```dart
class UpiPayment {
  final String upiId; final String? name; final int? amountPaise; final String? note;
  UpiPayment(this.upiId, this.name, this.amountPaise, this.note);
}

UpiPayment? parseUpi(String raw) {
  final uri = Uri.tryParse(raw);
  if (uri == null || uri.scheme != 'upi' || uri.host != 'pay') return null;
  final p = uri.queryParameters; // already decoded
  final pa = p['pa'];
  if (pa == null || !RegExp(r'^[\w.\-]+@[\w]+$').hasMatch(pa)) return null;
  int? paise;
  final am = p['am'];
  if (am != null) {
    if (!RegExp(r'^\d+(\.\d{1,2})?$').hasMatch(am)) return null;
    paise = (double.parse(am) * 100).round();
    if (paise <= 0) return null;
  }
  return UpiPayment(pa, p['pn'], paise, p['tn']);
}
```

**Follow-ups:** test cases you'd write (missing `pa`, `am=abc`, `am=-5`, `am=10.555`, wrong scheme); why `.round()`; trusting the payee name shown to the user (phishing risk).

---

## FL-L2: Pay button with state management

**Time:** 20 min · **Type:** coding (Flutter)

**Prompt**

> Build a `PayButton` widget that takes `Future<void> Function(String requestId) onPay`.
> - Shows "Pay", then a spinner while paying (button disabled), then "Paid ✓" or an error with "Retry".
> - Double taps never call `onPay` twice.
> - Retry reuses the same `requestId`.
> Use `StatefulWidget`, or Bloc / Riverpod if you prefer.

**Answer key points**

- A state enum: `idle, loading, success, error`.
- `requestId` created once (in `initState`) and reused on retry.
- Guard: `if (_state == PayState.loading) return;` at the top of the handler.
- `if (!mounted) return;` after `await` before `setState`.
- `onPressed: _state == PayState.loading ? null : _pay` to disable.
- **Follow-ups:** why the `mounted` check; how you'd move the logic into a Cubit and test it without UI; what happens if the app is killed mid-payment (need to persist `requestId` and check status on restart).

---

## FL-L3: Retry with timeout in Dart

**Time:** 15 min · **Type:** coding (Dart)

**Prompt**

> Write `Future<T> withRetry<T>(Future<T> Function() call, {int retries = 2, Duration timeout = const Duration(seconds: 5)})`.
> Each attempt times out after `timeout`. On `TimeoutException` or `SocketException`, wait `500ms × 2^attempt` and retry. Other errors are thrown immediately.

**Answer key**

```dart
Future<T> withRetry<T>(Future<T> Function() call,
    {int retries = 2, Duration timeout = const Duration(seconds: 5)}) async {
  for (var attempt = 0; ; attempt++) {
    try {
      return await call().timeout(timeout);
    } on TimeoutException {
      if (attempt >= retries) rethrow;
    } on SocketException {
      if (attempt >= retries) rethrow;
    }
    await Future.delayed(Duration(milliseconds: 500 * (1 << attempt)));
  }
}
```

**Follow-ups:** a timeout doesn't mean the server didn't process the payment. How do you retry safely? (Same request ID / idempotency key; or check the status first.)

---

## FL-L4: Code review of a widget

**Time:** 10–15 min · **Type:** code review

**Prompt**

> Find the problems in this widget.

**Starter code**

```dart
class BalanceScreen extends StatefulWidget {
  @override
  State<BalanceScreen> createState() => _BalanceScreenState();
}

class _BalanceScreenState extends State<BalanceScreen> {
  double balance = 0;
  final prefs = SharedPreferences.getInstance();

  @override
  Widget build(BuildContext context) {
    http.get(Uri.parse('https://api.example.com/balance')).then((r) {
      setState(() => balance = jsonDecode(r.body)['balance']);
    });
    Timer.periodic(Duration(seconds: 5), (_) => setState(() {}));
    return Text('Balance: ₹$balance');
  }
}
```

**Answer key**

1. Network call inside `build()`: runs on every rebuild → infinite request loop (each `setState` triggers `build`). Move to `initState`.
2. `Timer.periodic` created in `build`: a new timer every rebuild, never cancelled → leak. Create it in `initState`, cancel it in `dispose`.
3. `setState` after `await` without a `mounted` check.
4. `double` for money; parse to paise (`int`).
5. No error handling or status-code check; no loading state.
6. `SharedPreferences.getInstance()` stored as a Future and never used. For tokens, use `flutter_secure_storage`.
7. No auth header; the API URL is hard-coded.
8. Missing `const` / `key`; business logic mixed into the UI (should be in a repository + state class).
