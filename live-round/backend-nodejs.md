# Backend (Node.js): Live Round Tasks

Paste only the **Prompt** and **Starter code** into the call chat. Answer keys are for the interviewer.
FS-L1 (idempotent transfer) and FS-L2 (Express code review) from [full-stack-mern.md](full-stack-mern.md) also work well for backend candidates.

---

## BE-L1: Sliding-window rate limiter

**Time:** 20 min · **Type:** coding (JavaScript / TypeScript)

**Prompt**

> Write `isAllowed(userId, nowMs)` that returns `true` if the user has made **fewer than 5 requests in the last 60 seconds**, and records the request if allowed.

**Answer key**

```js
const hits = new Map(); // userId -> array of timestamps

function isAllowed(userId, now = Date.now()) {
  const windowStart = now - 60_000;
  const list = (hits.get(userId) || []).filter(t => t > windowStart);
  if (list.length >= 5) { hits.set(userId, list); return false; }
  list.push(now);
  hits.set(userId, list);
  return true;
}
```

**Follow-ups**

- Fixed window vs sliding window: what goes wrong with a fixed window at 0:59 and 1:01?
- Memory grows with users. How do you clean up?
- With 5 servers: Redis sorted set (`ZADD`, `ZREMRANGEBYSCORE`, `ZCARD`) inside a `MULTI` or a Lua script for atomicity.

---

## BE-L2: Retry with exponential backoff

**Time:** 15 min · **Type:** coding

**Prompt**

> Write `retry(fn, { retries = 3, baseMs = 200 })`. It calls the async `fn`; on failure it waits `baseMs × 2^attempt` (plus random jitter) and tries again, up to `retries` times. Only retry if the error has `error.retryable === true`.

**Answer key**

```js
const sleep = ms => new Promise(r => setTimeout(r, ms));

async function retry(fn, { retries = 3, baseMs = 200 } = {}) {
  for (let attempt = 0; ; attempt++) {
    try {
      return await fn(attempt);
    } catch (err) {
      if (!err.retryable || attempt >= retries) throw err;
      const delay = baseMs * 2 ** attempt + Math.random() * baseMs;
      await sleep(delay);
    }
  }
}
```

**Follow-ups**

- Why jitter? (Stops many clients retrying at the same moment.)
- Which payment API errors are retryable? (Timeouts, 502/503/504, 429. Not 400 or "insufficient balance".)
- Retrying a payment call that timed out: what must be true to make it safe? (Idempotency key on the request.)

---

## BE-L3: Verify a webhook signature

**Time:** 10–15 min · **Type:** coding

**Prompt**

> A payment provider sends webhooks with header `X-Signature` = hex HMAC-SHA256 of the **raw request body** using a shared secret. Write an Express handler that rejects requests with a wrong signature, and ignores an `eventId` already processed.

**Answer key**

```js
const crypto = require("crypto");

app.post("/webhooks/provider", express.raw({ type: "application/json" }), async (req, res) => {
  const expected = crypto.createHmac("sha256", process.env.WEBHOOK_SECRET).update(req.body).digest("hex");
  const given = req.get("X-Signature") || "";
  const ok = given.length === expected.length &&
             crypto.timingSafeEqual(Buffer.from(given), Buffer.from(expected));
  if (!ok) return res.status(401).end();

  const event = JSON.parse(req.body);
  const inserted = await ProcessedEvent.updateOne(
    { eventId: event.eventId }, { $setOnInsert: { at: new Date() } }, { upsert: true });
  if (inserted.upsertedCount === 0) return res.status(200).end(); // duplicate: ack, do nothing

  await handleEvent(event);
  res.status(200).end();
});
```

**What to look for:** uses the **raw** body (not re-stringified JSON); constant-time comparison; returns 200 for duplicates; unique index on `eventId`.
**Follow-ups:** what if `handleEvent` fails after the event was marked processed? (Mark after success, or use a status field, or a queue.) How to prevent replay of old events? (A timestamp in the signed payload.)

---

## BE-L4: System design mini-question

**Time:** 15 min · **Type:** discussion with a quick diagram

**Prompt**

> Design the flow for a wallet top-up through a payment gateway: the user pays on the gateway, gets redirected back, and the gateway also sends a webhook. Draw the components and the order of calls. How do you make sure the wallet is credited exactly once?

**Strong answer includes**

- Order created as `CREATED` with a unique order ID before redirecting.
- **Webhook is the source of truth**; the redirect only shows "confirming…".
- Credit the wallet and mark the order `SUCCESS` in **one transaction**, conditional on the order still being `PENDING`.
- Duplicate webhooks ignored (event ID or the conditional update).
- A reconciliation job queries the gateway for orders stuck in `PENDING`.
- Signature verification, and logs with a correlation ID.

---

## BE-L5: Process a huge settlement file with streams

**Time:** 20 min · **Type:** coding (Node.js streams)

**Prompt**

> The bank sends a **2 GB CSV** every night: `merchant_id,amount_paise,status`.
> Write a function that returns the total `SUCCESS` amount per merchant **without loading the whole file into memory**. Count and skip bad rows.

**Answer key**

```js
const fs = require("fs");
const readline = require("readline");

async function settlementTotals(path) {
  const rl = readline.createInterface({ input: fs.createReadStream(path), crlfDelay: Infinity });
  const totals = new Map();
  let lineNo = 0, bad = 0;
  for await (const line of rl) {
    if (lineNo++ === 0) continue;                       // header
    const [merchantId, amountStr, status] = line.split(",");
    const amount = Number(amountStr);
    if (!merchantId || !Number.isInteger(amount) || amount < 0) { bad++; continue; }
    if (status !== "SUCCESS") continue;
    totals.set(merchantId, (totals.get(merchantId) || 0) + amount);
  }
  return { totals: Object.fromEntries(totals), badRows: bad };
}
```

**Follow-ups**

- Why not `fs.readFileSync` + `split("\n")`? (2 GB in memory; may crash.)
- What is backpressure? When would you need `stream.pipeline`? (Writing results to another stream or a DB faster than you can read.)
- Simple `split(",")` breaks on quoted values like `"Shop, Bhopal"`. What would you use? (A streaming CSV parser such as `csv-parse`.)
- Totals for 1 million merchants: still fine in a `Map`? When would you write to the database in batches instead?

---

## BE-L6: In-memory job queue with retries and a dead-letter list

**Time:** 20 min · **Type:** coding (async JavaScript)

**Prompt**

> Build a small `JobQueue` for sending payment notifications:
> - `new JobQueue(handler, { concurrency: 2, maxAttempts: 3 })`
> - `add(job)` adds a job; at most `concurrency` jobs run at once.
> - A failed job is retried with backoff (`100ms × 2^attempt`). After `maxAttempts` it goes to a `dead` list with the last error.

**Answer key**

```js
class JobQueue {
  constructor(handler, { concurrency = 2, maxAttempts = 3 } = {}) {
    Object.assign(this, { handler, concurrency, maxAttempts });
    this.queue = []; this.dead = []; this.running = 0;
  }
  add(job) { this.queue.push({ job, attempts: 0 }); this.drain(); }
  drain() {
    while (this.running < this.concurrency && this.queue.length) {
      const item = this.queue.shift();
      this.running++;
      Promise.resolve()
        .then(() => this.handler(item.job))
        .catch(err => {
          item.attempts++; item.lastError = err.message;
          if (item.attempts < this.maxAttempts) {
            setTimeout(() => { this.queue.push(item); this.drain(); }, 100 * 2 ** item.attempts);
          } else {
            this.dead.push(item);
          }
        })
        .finally(() => { this.running--; this.drain(); });
    }
  }
}
```

**Follow-ups**

- What happens to queued jobs if the server restarts? (Lost. Use a persistent queue: BullMQ with Redis, RabbitMQ, SQS.)
- Notifications may be sent twice after a crash. How do you make the handler safe to run twice? (Idempotent handler: store a "sent" marker per notification ID.)
- How would you monitor the dead-letter list?

---

## BE-L7: First successful provider, with a timeout

**Time:** 15 min · **Type:** coding (promises)

**Prompt**

> We fetch the INR/USD exchange rate from 3 providers. Write `getRate(providers)` where each provider is an async function. Return the **first successful** result. Each provider gets at most **2 seconds**. If all fail, throw `ALL_PROVIDERS_FAILED`.

**Answer key**

```js
function withTimeout(promise, ms) {
  let timer;
  const timeout = new Promise((_, reject) => { timer = setTimeout(() => reject(new Error("TIMEOUT")), ms); });
  return Promise.race([promise, timeout]).finally(() => clearTimeout(timer));
}

async function getRate(providers) {
  try {
    return await Promise.any(providers.map(p => withTimeout(p(), 2000)));
  } catch (e) {               // AggregateError when all reject
    throw new Error("ALL_PROVIDERS_FAILED");
  }
}
```

**Follow-ups**

- `Promise.all` vs `allSettled` vs `race` vs `any`: when do you use each?
- Why `clearTimeout`? (Otherwise timers pile up and keep the process busy.)
- **Fintech twist:** would you use this pattern to **send a payment** to 3 payment providers at once? (No! You'd be charged up to 3 times. Racing is only for safe, read-only calls.)
- The timed-out request keeps running in the background. How would you really cancel it? (`AbortController` with `fetch`.)

---

## BE-L8: Payment state machine

**Time:** 15 min · **Type:** coding

**Prompt**

> Write `transition(payment, toStatus, reason)` for these rules:
> `CREATED → PENDING | FAILED`, `PENDING → SUCCESS | FAILED | EXPIRED`, `SUCCESS → REFUNDED`. Final states: `FAILED`, `EXPIRED`, `REFUNDED`.
> - Invalid moves throw an error with HTTP status 409.
> - Moving to the status it already has is a no-op (webhooks repeat).
> - Every change is appended to `payment.history` with from, to, reason and time.

**Answer key**

```js
const ALLOWED = {
  CREATED: ["PENDING", "FAILED"],
  PENDING: ["SUCCESS", "FAILED", "EXPIRED"],
  SUCCESS: ["REFUNDED"],
  FAILED: [], EXPIRED: [], REFUNDED: [],
};

function transition(payment, to, reason) {
  if (payment.status === to) return payment;                       // idempotent repeat
  if (!(ALLOWED[payment.status] || []).includes(to)) {
    throw Object.assign(new Error(`Invalid transition ${payment.status} -> ${to}`), { status: 409 });
  }
  return {
    ...payment,
    status: to,
    history: [...payment.history, { from: payment.status, to, reason, at: new Date().toISOString() }],
  };
}
```

**Follow-ups**

- Two webhooks (`SUCCESS` and `FAILED`) arrive at the same moment on two servers. Both read `PENDING`. How do you make the database enforce the rule? (Conditional update: `updateOne({ _id, status: "PENDING" }, { $set: { status: "SUCCESS" }, $push: { history: … } })` and check `modifiedCount`.)
- Should `FAILED → SUCCESS` ever be allowed? (Discuss: late success from a bank after a timeout. Usually handled by reconciliation and a refund, not a status flip.)

---

## BE-L9: Find the memory leaks

**Time:** 10–15 min · **Type:** code review

**Prompt**

> After a few hours in production, this service uses 4 GB of memory and logs `MaxListenersExceededWarning`. Find every problem.

**Starter code**

```js
const cache = {};
const emitter = require("./rateEvents");

app.get("/rates/:currency", async (req, res) => {
  const key = req.params.currency + Date.now();
  if (!cache[key]) cache[key] = await fetchRate(req.params.currency);

  process.on("exit", () => console.log("served", key));
  emitter.on("rateChanged", rate => res.write(JSON.stringify(rate)));

  res.json(cache[key]);
});
```

**Answer key**

1. The cache key includes `Date.now()`: every request is a miss and a new entry → the cache grows forever.
2. No eviction or TTL even with a correct key. Use an LRU with TTL, or Redis with `EX`.
3. `process.on("exit")` added on **every request** → listener leak (the warning) and each closure holds `key` in memory.
4. `emitter.on("rateChanged")` added per request and never removed → the closure keeps `res` alive forever (the main leak).
5. `res.write` after `res.json` has ended the response → "write after end" errors.
6. No input validation on `currency` (unbounded keys from user input).
7. No error handling around `fetchRate`.
8. Many parallel requests for the same currency all call `fetchRate` (cache stampede). Store the promise, not the value.

**Follow-ups:** how you'd confirm a leak in production (`--inspect` + heap snapshots, `process.memoryUsage()`, clinic.js); how to send live rate updates properly (SSE or WebSocket with cleanup on `req.on("close")`).

---

## BE-L10: Atomic debit and transfer in MongoDB

**Time:** 15–20 min · **Type:** coding (MongoDB / Mongoose)

**Prompt**

> 1. Write `debit(userId, amountPaise)` that is safe when 10 requests run at the same moment, **without** using a transaction. The balance must never go negative.
> 2. Write `transfer(fromId, toId, amountPaise, idempotencyKey)` that moves money between two wallets atomically and never runs twice for the same key.

**Answer key**

```js
// 1. Single-document atomic update
async function debit(userId, amount) {
  const wallet = await Wallet.findOneAndUpdate(
    { userId, balance: { $gte: amount } },
    { $inc: { balance: -amount } },
    { new: true }
  );
  if (!wallet) throw Object.assign(new Error("INSUFFICIENT_BALANCE_OR_NOT_FOUND"), { status: 422 });
  return wallet.balance;
}

// 2. Multi-document transaction (needs a replica set)
async function transfer(fromId, toId, amount, key) {
  const session = await mongoose.startSession();
  try {
    return await session.withTransaction(async () => {
      await Transfer.create([{ idempotencyKey: key, fromId, toId, amount }], { session }); // unique index on key
      const from = await Wallet.findOneAndUpdate(
        { userId: fromId, balance: { $gte: amount } }, { $inc: { balance: -amount } }, { new: true, session });
      if (!from) throw new Error("INSUFFICIENT_BALANCE");
      const to = await Wallet.updateOne({ userId: toId }, { $inc: { balance: amount } }, { session });
      if (to.matchedCount !== 1) throw new Error("RECEIVER_NOT_FOUND");
      return from.balance;
    });
  } catch (e) {
    if (e.code === 11000) return getExistingTransferResult(key); // duplicate key → already processed
    throw e;
  } finally {
    await session.endSession();
  }
}
```

**Follow-ups**

- Why does `findOne` + check + `save()` fail under concurrency? (Two requests read the same balance before either writes.)
- What does `withTransaction` do on a transient error? (Retries the whole callback, which is why the callback must be safe to re-run.)
- What if MongoDB is a standalone server? (Transactions are unavailable; use a single-document design such as a ledger entry plus balance in one document, or a two-phase pattern.)

---

## BE-L11: The API slows down when reports run

**Time:** 10 min · **Type:** discussion

**Prompt**

> Our Node.js API is fast, but when someone downloads a monthly PDF statement (or we hash passwords with a high bcrypt cost), **all** other requests slow down for a few seconds. Why, and how would you fix it?

**Strong answer includes**

- Node runs JavaScript on **one thread**; CPU-heavy work blocks the event loop, so every request waits.
- How to confirm: event-loop lag (`perf_hooks.monitorEventLoopDelay`), CPU profiling (`--prof`, clinic.js).
- Fixes:
  - move heavy work to **`worker_threads`** (or a pool like Piscina)
  - or to a **background job** (queue + worker service) that emails or uploads the PDF when ready
  - use the async `bcrypt.hash` (runs in the libuv thread pool; size via `UV_THREADPOOL_SIZE`)
  - run several processes to use all CPU cores (cluster mode, PM2, or multiple containers)
- Streaming the PDF instead of building it fully in memory.

**Red flag:** "make it async" with no idea that `async` doesn't move CPU work off the main thread.
