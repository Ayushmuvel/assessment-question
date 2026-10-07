# QA Engineer: Live Round Tasks

Paste only the **Prompt** (and any **Starter** material) into the call chat. Answer keys are for the interviewer.
The candidate can type into a shared doc, a spreadsheet, or their own editor.

---

## QA-L1: Rapid test design: UPI collect request

**Time:** 15 min · **Type:** test design

**Prompt**

> A merchant sends a UPI "collect request" to a customer. The customer gets a notification and can **approve with their UPI PIN** or **decline**. The request expires after **10 minutes**. Amount limit: ₹1 to ₹1,00,000.
> List as many test cases as you can in 12 minutes. Then mark your top 5.

**Answer key: strong candidates cover**

- Approve with correct PIN → both balances and statuses updated.
- Wrong PIN (once, 3 times → lock), decline, ignore until expiry.
- Approve at 9:59 vs 10:01; approve after decline.
- Boundaries: ₹0, ₹1, ₹1,00,000, ₹1,00,001, decimals (₹10.005).
- Insufficient balance; account blocked; invalid / inactive UPI ID.
- **Duplicate:** approve tapped twice; same request sent twice by the merchant.
- **Failure:** network drops after PIN entry; bank timeout → status Pending → final status later; money debited but merchant not notified.
- Notifications: customer offline, app killed, notification arrives late.
- Merchant side: status updates, refund after success.
- Security: someone else's collect request; tampered amount.

**Scoring:** 20+ relevant cases with duplicate and failure scenarios = 4; mostly happy-path and basic negatives = 2.

---

## QA-L2: Live SQL

**Time:** 15 min · **Type:** query writing

**Prompt**

> Tables:
> `wallets(user_id, balance)`
> `transactions(id, user_id, type, amount, status, created_at)` with type `CREDIT` / `DEBIT`
> 1. Find users with more than 3 `FAILED` transactions today.
> 2. Find users whose `wallets.balance` doesn't match their successful credits minus debits.
> 3. Find the 5 largest successful debits this month with the user ID.

**Answer key**

```sql
-- 1
SELECT user_id, COUNT(*) AS failed
FROM transactions
WHERE status = 'FAILED' AND created_at >= CURRENT_DATE
GROUP BY user_id HAVING COUNT(*) > 3;

-- 2
SELECT w.user_id, w.balance, COALESCE(t.net, 0) AS calculated
FROM wallets w
LEFT JOIN (
  SELECT user_id, SUM(CASE WHEN type = 'CREDIT' THEN amount ELSE -amount END) AS net
  FROM transactions WHERE status = 'SUCCESS' GROUP BY user_id
) t ON t.user_id = w.user_id
WHERE w.balance <> COALESCE(t.net, 0);

-- 3
SELECT id, user_id, amount FROM transactions
WHERE type = 'DEBIT' AND status = 'SUCCESS'
  AND created_at >= DATE_TRUNC('month', CURRENT_DATE)
ORDER BY amount DESC LIMIT 5;
```

**Look for:** `HAVING` vs `WHERE`; `LEFT JOIN` + `COALESCE` (users with no transactions); correct date boundaries.

---

## QA-L3: Find the problems in a requirement

**Time:** 10 min · **Type:** requirement review

**Prompt**

> "Users can get a refund for any transaction. Refunds are processed quickly and the user is informed. Large refunds need approval."
> What questions would you ask the product owner before writing test cases?

**Answer key: good questions**

- Which transactions: failed or pending ones? How old (time limit)?
- Full and partial refunds? Multiple partial refunds? Can the total exceed the original?
- What does "quickly" mean (SLA)? Instant to wallet, or 5–7 days to bank?
- How is the user informed (SMS, email, push)? What if it fails?
- What counts as "large"? Who approves? What if rejected? Can the requester approve their own?
- What happens to fees and cashback on refund?
- Duplicate refund requests? Refund while the original is still pending?
- Reports and audit trail?

**Scoring:** 8+ sharp questions = 4. Starts writing test cases without asking anything = 1–2.

---

## QA-L4: Bug report and API check from a scenario

**Time:** 15 min · **Type:** bug report + API thinking

**Prompt**

> You call `POST /api/transfer` with `{"to":"U2","amount":500}` and header `Idempotency-Key: abc`. Response: `200 {"status":"SUCCESS","txnId":"T1"}`. You send the **exact same request again**. Response: `200 {"status":"SUCCESS","txnId":"T2"}`. The sender's balance dropped by ₹1,000.
> 1. Write the bug report.
> 2. What else would you test around this endpoint?
> 3. Write the Postman test script (or describe the assertions) that would catch this bug automatically.

**Answer key**

- **Bug report:** title like "Duplicate debit when the same Idempotency-Key is reused on /api/transfer"; steps; expected (second call returns T1, balance drops ₹500 once); actual; severity **Critical**, priority **High**; evidence (requests, responses, balance before and after, txn IDs).
- **More tests:** same key with a different amount; parallel requests with the same key; missing key; key reuse after 24 hours; different users with the same key.
- **Postman:**

```js
pm.test("Same key returns the same transaction", () => {
  const first = pm.environment.get("firstTxnId");
  pm.expect(pm.response.json().txnId).to.eql(first);
});
```

  plus a balance check via `GET /api/wallet` before and after.
