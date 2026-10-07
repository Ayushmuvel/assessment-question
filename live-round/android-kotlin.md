# Android Developer (Kotlin): Live Round Tasks

Paste only the **Prompt** and **Starter code** into the call chat. Answer keys are for the interviewer.
The Kotlin Playground (play.kotlinlang.org) works for AND-L1 and AND-L4 if Android Studio is slow to start.

---

## AND-L1: Retry with backoff using coroutines

**Time:** 15 min · **Type:** coding (Kotlin)

**Prompt**

> Write `suspend fun <T> retryIo(times: Int = 3, initialDelayMs: Long = 500, block: suspend () -> T): T`.
> Retry only on `IOException`, doubling the delay each time. Other exceptions are thrown immediately. It must respect coroutine cancellation.

**Answer key**

```kotlin
suspend fun <T> retryIo(times: Int = 3, initialDelayMs: Long = 500, block: suspend () -> T): T {
    var delayMs = initialDelayMs
    repeat(times - 1) {
        try {
            return block()
        } catch (e: IOException) {
            // retry
        }
        delay(delayMs)          // delay() is cancellable
        delayMs *= 2
    }
    return block()              // last attempt: let the exception propagate
}
```

**Follow-ups**

- Why not catch `Exception`? (It would swallow `CancellationException` and break cancellation.)
- Which dispatcher does this run on? (The caller's; network calls should be on `Dispatchers.IO`, which Retrofit's suspend functions already handle.)
- Retrying a payment call: what makes it safe? (The same `requestId` on every attempt.)

---

## AND-L2: Payment ViewModel with StateFlow

**Time:** 20 min · **Type:** coding (Kotlin, Android)

**Prompt**

> Write a `PaymentViewModel` with:
> - a `sealed interface PaymentUiState` (`Idle`, `Processing`, `Success(txnId)`, `Error(message)`),
> - `val state: StateFlow<PaymentUiState>`,
> - `fun pay(amountPaise: Long)` that calls `repository.pay(requestId, amountPaise)`.
> Calling `pay` twice while processing must not start a second payment. Retry after an error must reuse the same `requestId`.

**Answer key**

```kotlin
sealed interface PaymentUiState {
    data object Idle : PaymentUiState
    data object Processing : PaymentUiState
    data class Success(val txnId: String) : PaymentUiState
    data class Error(val message: String) : PaymentUiState
}

class PaymentViewModel(
    private val repository: PaymentRepository,
    private val savedState: SavedStateHandle
) : ViewModel() {
    private val _state = MutableStateFlow<PaymentUiState>(PaymentUiState.Idle)
    val state: StateFlow<PaymentUiState> = _state.asStateFlow()

    private val requestId: String =
        savedState.get<String>("requestId") ?: UUID.randomUUID().toString().also { savedState["requestId"] = it }

    fun pay(amountPaise: Long) {
        if (_state.value is PaymentUiState.Processing) return
        _state.value = PaymentUiState.Processing
        viewModelScope.launch {
            _state.value = try {
                PaymentUiState.Success(repository.pay(requestId, amountPaise))
            } catch (e: IOException) {
                PaymentUiState.Error("Network error. Please retry.")
            }
        }
    }
}
```

**Follow-ups:** why `SavedStateHandle` (process death); how you'd collect the state in Compose (`collectAsStateWithLifecycle`); how to unit test it (fake repository + `runTest` + `StandardTestDispatcher`).

---

## AND-L3: Code review of an Activity

**Time:** 15 min · **Type:** code review

**Prompt**

> This screen is in a payments app. Find the problems.

**Starter code**

```kotlin
object Session { lateinit var activity: Activity }

class PayActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        Session.activity = this
        val pin = intent.getStringExtra("pin")
        Log.d("PAY", "Paying with pin $pin")

        GlobalScope.launch {
            val result = api.pay(amount = 100.50, pin = pin!!)
            findViewById<TextView>(R.id.status).text = result.status
        }
        getSharedPreferences("app", MODE_PRIVATE).edit().putString("token", api.token).apply()
    }
}
```

**Answer key**

1. `Session.activity = this`: static reference to an Activity → memory leak after rotation or close.
2. **PIN logged** in Logcat: a serious security issue. Never log PINs, OTPs, or tokens.
3. PIN passed through an Intent extra: should not travel between screens in plain text.
4. `GlobalScope`: not tied to the lifecycle, keeps running after the screen closes. Use `lifecycleScope` / `viewModelScope`.
5. UI updated from a background coroutine (not the main thread) → crash.
6. Logic in the Activity: rotation restarts the payment. Move it to a ViewModel.
7. `100.50` as a Double for money: use paise (`Long`).
8. `pin!!` can crash.
9. Token in plain `SharedPreferences`: use EncryptedSharedPreferences or DataStore + Keystore.
10. No `FLAG_SECURE` on a payment screen; no error handling; no idempotency / requestId.

**Scoring:** 6+ with fixes = 4; 4–5 = 3.

---

## AND-L4: Amount and card validation

**Time:** 10 min · **Type:** coding (Kotlin)

**Prompt**

> 1. Write `fun parseAmountToPaise(input: String): Long?`: accepts `"120"`, `"120.5"`, `"120.50"`, rejects `"0"`, `"-5"`, `"12.345"`, `"abc"`, and anything over ₹50,000.
> 2. Write `fun isValidCard(number: String): Boolean` using the Luhn algorithm.

**Answer key**

```kotlin
fun parseAmountToPaise(input: String): Long? {
    if (!Regex("""^\d+(\.\d{1,2})?$""").matches(input)) return null
    val paise = input.toBigDecimal().movePointRight(2).toLong()
    return paise.takeIf { it in 1..5_000_000 }
}

fun isValidCard(number: String): Boolean {
    val digits = number.filter { !it.isWhitespace() }
    if (digits.length !in 12..19 || !digits.all { it.isDigit() }) return false
    val sum = digits.reversed().mapIndexed { i, c ->
        val d = c - '0'
        if (i % 2 == 1) (d * 2).let { if (it > 9) it - 9 else it } else d
    }.sum()
    return sum % 10 == 0
}
```

**Follow-ups:** why `BigDecimal` instead of `toDouble() * 100`; test cases for both; where this validation belongs (domain layer, shared by UI and tests).
