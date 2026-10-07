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

---

## FS-L5: Predict the output (event loop)

**Time:** 10 min · **Type:** Node.js fundamentals

**Prompt**

> Without running it, write down the order in which the numbers are printed. Explain why.

**Starter code**

```js
console.log("1");
setTimeout(() => console.log("2"), 0);
setImmediate(() => console.log("3"));
Promise.resolve().then(() => console.log("4"));
process.nextTick(() => console.log("5"));
(async () => {
  console.log("6");
  await null;
  console.log("7");
})();
console.log("8");
```

**Answer key**

- `1, 6, 8, 5, 4, 7`, then `2` and `3`.
- Synchronous code first (1, 6, 8; the async function runs synchronously until its first `await`).
- `process.nextTick` queue runs before promise microtasks (5).
- Promise microtasks in the order they were queued (4, then 7).
- `setTimeout(0)` vs `setImmediate` order is **not guaranteed** in the main module. Inside an I/O callback, `setImmediate` always runs first.

**Follow-ups:** why a long synchronous loop delays every timer; why recursive `process.nextTick` can starve I/O.

**Scoring:** correct order with the reasons = 4; correct sync part, mixes up microtasks = 2.

---

## FS-L6: Run payouts with a concurrency limit

**Time:** 20 min · **Type:** coding (async JavaScript)

**Prompt**

> You have 100 merchant payouts. The bank API allows at most **5 requests at the same time**.
> Write `mapLimit(items, limit, fn)` that runs the async `fn` on every item with at most `limit` running at once, and returns results **in the original order**. One failure must not stop the others.

**Answer key**

```js
async function mapLimit(items, limit, fn) {
  const results = new Array(items.length);
  let next = 0;
  async function worker() {
    while (next < items.length) {
      const i = next++;
      try { results[i] = { status: "fulfilled", value: await fn(items[i], i) }; }
      catch (e) { results[i] = { status: "rejected", reason: e }; }
    }
  }
  await Promise.all(Array.from({ length: Math.min(limit, items.length) }, worker));
  return results;
}
```

**Follow-ups**

- Why not `Promise.all(items.map(fn))`? (All 100 start at once and the bank rejects them.)
- Why is `next++` safe without a lock? (JavaScript runs one piece of code at a time; there is no `await` between reading and incrementing.)
- A payout times out. Is it safe to retry it? (Only with an idempotency key; otherwise the merchant may be paid twice.)

---

## FS-L7: JWT auth and role middleware

**Time:** 15 min · **Type:** coding (Express)

**Prompt**

> Write two Express middlewares:
> 1. `authenticate`: reads `Authorization: Bearer <token>`, verifies the JWT, and puts the user on `req.user`. Returns 401 if missing or invalid.
> 2. `requireRole(...roles)`: returns 403 if `req.user.role` is not one of the roles.
> Show how you would protect `POST /refunds/:id/approve` so only `supervisor` and `admin` can call it.

**Answer key**

```js
const jwt = require("jsonwebtoken");

function authenticate(req, res, next) {
  const header = req.headers.authorization || "";
  const token = header.startsWith("Bearer ") ? header.slice(7) : null;
  if (!token) return res.status(401).json({ error: "UNAUTHENTICATED" });
  try {
    req.user = jwt.verify(token, process.env.JWT_SECRET, { algorithms: ["HS256"] });
    next();
  } catch {
    res.status(401).json({ error: "INVALID_TOKEN" });
  }
}

const requireRole = (...roles) => (req, res, next) =>
  roles.includes(req.user?.role) ? next() : res.status(403).json({ error: "FORBIDDEN" });

app.post("/refunds/:id/approve", authenticate, requireRole("supervisor", "admin"), approveRefund);
```

**Follow-ups**

- 401 vs 403: when is each correct?
- Why pin `algorithms`? (Prevents algorithm-confusion attacks such as `alg: none`.)
- Role checks are not enough: a supervisor must not approve their own refund. Where does that check go? (In the service, comparing `refund.requestedBy` with `req.user.id`.)
- How do you log a user out before the token expires? (Short expiry + refresh tokens, or a deny-list in Redis.)

---

## FS-L8: Mongoose schema design

**Time:** 15 min · **Type:** data modelling

**Prompt**

> Design Mongoose schemas for `Wallet` and `Transaction` in a wallet app.
> - One wallet per user; the balance can never go negative.
> - Transactions: credit or debit, amount, status, optional idempotency key that must be unique when present.
> - The most common query is "latest 20 transactions for a wallet".
> Add the validation and indexes you think are needed.

**Answer key**

```js
const walletSchema = new Schema({
  userId:   { type: Schema.Types.ObjectId, ref: "User", required: true, unique: true },
  balance:  { type: Number, required: true, min: 0, validate: { validator: Number.isInteger } }, // paise
  currency: { type: String, default: "INR", enum: ["INR"] },
}, { timestamps: true });

const transactionSchema = new Schema({
  walletId:       { type: Schema.Types.ObjectId, ref: "Wallet", required: true },
  type:           { type: String, enum: ["CREDIT", "DEBIT"], required: true },
  amount:         { type: Number, required: true, min: 1, validate: { validator: Number.isInteger } },
  status:         { type: String, enum: ["PENDING", "SUCCESS", "FAILED"], default: "PENDING" },
  idempotencyKey: { type: String },
  reference:      { type: String },
}, { timestamps: true });

transactionSchema.index({ walletId: 1, createdAt: -1 });
transactionSchema.index({ idempotencyKey: 1 }, { unique: true, partialFilterExpression: { idempotencyKey: { $type: "string" } } });
```

**Follow-ups**

- Does `min: 0` stop two parallel debits from overdrawing? (No: validators run on the document, not atomically. Use a conditional update `{ balance: { $gte: amount } }`.)
- Why integer paise instead of `Decimal128`? Both are fine if justified.
- Embed transactions in the wallet document? (No: unbounded growth, 16 MB document limit.)

---

## FS-L9: Async errors in Express

**Time:** 10 min · **Type:** debugging

**Prompt**

> In production, this route sometimes makes the request hang until it times out, and once it crashed the whole server. Why? Fix it so every error returns a clean JSON response.

**Starter code**

```js
app.get("/wallet/:id", async (req, res) => {
  const wallet = await Wallet.findById(req.params.id);
  res.json({ balance: wallet.balance });
});
```

**Answer key**

- An invalid ID (`CastError`) or a null wallet throws inside an `async` handler. Express 4 does not catch rejected promises, so no response is sent (the request hangs) and an unhandled rejection can crash the process.
- Fix:

```js
const asyncHandler = fn => (req, res, next) => Promise.resolve(fn(req, res, next)).catch(next);

app.get("/wallet/:id", asyncHandler(async (req, res) => {
  if (!mongoose.isValidObjectId(req.params.id)) return res.status(400).json({ error: "INVALID_ID" });
  const wallet = await Wallet.findById(req.params.id);
  if (!wallet) return res.status(404).json({ error: "NOT_FOUND" });
  res.json({ balance: wallet.balance });
}));

app.use((err, req, res, next) => {
  const status = err.status || 500;
  logger.error({ err, path: req.path });
  res.status(status).json({ error: err.code || "INTERNAL_ERROR",
                            message: status < 500 ? err.message : "Something went wrong" });
});
```

**Follow-ups:** what Express 5 changes (it forwards rejected promises to `next`); should you `process.on("unhandledRejection")` and keep running? (Log it, then restart cleanly; the process may be in a bad state.); also, any user can read any wallet. Who should be allowed? (Owner check.)
