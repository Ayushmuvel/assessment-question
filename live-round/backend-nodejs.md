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
