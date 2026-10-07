# Full Stack (MERN): Live Round Tasks

Paste only the **Prompt** and **Starter code** into the call chat. Answer keys are for the interviewer.

---

## FS-L1: Idempotent transfer function

**Time:** 20 min · **Type:** coding (JavaScript / TypeScript)

**Prompt**

> Implement `transfer(fromId, toId, amount, idempotencyKey)` using the in-memory data below.
> - Reject: amount ≤ 0, transfer to self, unknown wallet, insufficient balance.
> - If the same `idempotencyKey` is used again, return the **original result** without moving money.
> - Return `{ status: "SUCCESS" | "FAILED", reason?, balanceAfter? }`.

**Starter code**

```js
const wallets = { A: 10000, B: 5000 }; // balances in paise
const processed = new Map();          // idempotencyKey -> result

function transfer(fromId, toId, amount, idempotencyKey) {
  // your code
}
```

**Answer key**

```js
function transfer(fromId, toId, amount, key) {
  if (processed.has(key)) return processed.get(key);
  let result;
  if (!Number.isInteger(amount) || amount <= 0) result = { status: "FAILED", reason: "INVALID_AMOUNT" };
  else if (fromId === toId) result = { status: "FAILED", reason: "SAME_WALLET" };
  else if (!(fromId in wallets) || !(toId in wallets)) result = { status: "FAILED", reason: "UNKNOWN_WALLET" };
  else if (wallets[fromId] < amount) result = { status: "FAILED", reason: "INSUFFICIENT_BALANCE" };
  else {
    wallets[fromId] -= amount;
    wallets[toId] += amount;
    result = { status: "SUCCESS", balanceAfter: wallets[fromId] };
  }
  processed.set(key, result);
  return result;
}
```

**Follow-ups**

- Should a FAILED result also be stored under the key? (Discuss: yes for validation failures; maybe not for temporary errors, so the client can retry.)
- Same key, different amount: what should happen? (Reject with a conflict error.)
- This is in memory. How does it change with MongoDB and two servers? (Unique index on the key, atomic `$inc` with a balance condition, or a transaction.)

---

## FS-L2: Code review of an Express route

**Time:** 15 min · **Type:** code review

**Prompt**

> This route is about to go to production. Find as many problems as you can and say how you'd fix each.

**Starter code**

```js
app.post("/transfer", async (req, res) => {
  const { from, to, amount } = req.body;
  const sender = await Wallet.findOne({ userId: from });
  if (sender.balance > amount) {
    sender.balance = sender.balance - parseFloat(amount);
    sender.save();
    await Wallet.updateOne({ userId: to }, { $inc: { balance: amount } });
    res.json({ ok: true, balance: sender.balance });
  }
  res.json({ ok: false });
});
```

**Answer key (look for at least 6)**

1. No authentication: `from` comes from the body, so anyone can send money from any wallet. Take it from the JWT.
2. No validation: negative, zero, or string amounts; `to` might not exist; transfer to self.
3. `>` should be `>=` (exact balance is allowed).
4. Floating point money (`parseFloat`). Use integer paise.
5. `sender.save()` is not awaited; errors are lost and the order is not guaranteed.
6. Race condition: read-then-write; two parallel requests can overdraw. Use an atomic conditional update (`{ balance: { $gte: amount } }` with `$inc`) or a transaction.
7. Debit and credit are not atomic: a crash between them loses money. Use a transaction.
8. No idempotency key: a client retry moves money twice.
9. Two responses sent on success (`res.json` twice) → "headers already sent" error; missing `return`.
10. `amount` in `$inc` may be a string from the body.
11. No error handling (`sender` may be null → crash).

**Scoring:** 6+ with good fixes = 4; 4–5 = 3; 2–3 = 2.

---

## FS-L3: Pay button component

**Time:** 15–20 min · **Type:** coding (React)

**Prompt**

> Build a `PayButton` component that takes `amount` and an async `onPay(idempotencyKey)` function.
> - While paying: the button is disabled and shows "Processing…".
> - On success: show "Paid ✓". On error: show the error message and allow retry.
> - Double-clicking must never call `onPay` twice.
> - A retry after an error must reuse the same idempotency key.

**Answer key**

```jsx
function PayButton({ amount, onPay }) {
  const [state, setState] = useState("idle"); // idle | loading | success | error
  const [error, setError] = useState(null);
  const keyRef = useRef(crypto.randomUUID());
  const inFlight = useRef(false);

  async function handleClick() {
    if (inFlight.current) return;      // guards against double click before re-render
    inFlight.current = true;
    setState("loading"); setError(null);
    try {
      await onPay(keyRef.current);
      setState("success");
    } catch (e) {
      setError(e.message); setState("error");
    } finally {
      inFlight.current = false;
    }
  }

  if (state === "success") return <p>Paid ✓</p>;
  return (
    <>
      <button onClick={handleClick} disabled={state === "loading"}>
        {state === "loading" ? "Processing…" : `Pay ₹${(amount / 100).toFixed(2)}`}
      </button>
      {error && <p role="alert">{error}</p>}
    </>
  );
}
```

**Follow-ups**

- Why a `ref` and not only `disabled`? (Two clicks can fire before React re-renders.)
- When should a **new** idempotency key be generated? (A new payment intent, for example the amount changes.)
- What if the component unmounts mid-request?

---

## FS-L4: MongoDB query and index

**Time:** 10 min · **Type:** query writing

**Prompt**

> Collection `transactions`: `{ merchantId, amount, status, createdAt }`.
> Write a query that returns, for one merchant, the **daily total and count of SUCCESS transactions for the last 7 days**. Then say which index you would add.

**Answer key**

```js
db.transactions.aggregate([
  { $match: { merchantId: "M1", status: "SUCCESS",
              createdAt: { $gte: new Date(Date.now() - 7 * 864e5) } } },
  { $group: { _id: { $dateToString: { format: "%Y-%m-%d", date: "$createdAt", timezone: "Asia/Kolkata" } },
              total: { $sum: "$amount" }, count: { $sum: 1 } } },
  { $sort: { _id: 1 } }
]);
```

- Index: `{ merchantId: 1, status: 1, createdAt: 1 }` (equality fields first, then the range).
- **Follow-ups:** why the timezone matters (IST day boundaries); how to show days with zero transactions; how to check the index is used (`explain()`).
