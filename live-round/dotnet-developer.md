# .NET Developer: Live Round Tasks

Paste only the **Prompt** and **Starter code** into the call chat. Answer keys are for the interviewer.

---

## NET-L1: Thread-safe idempotent transfer service

**Time:** 20 min · **Type:** coding (C#)

**Prompt**

> Implement `TransferResult Transfer(string from, string to, decimal amount, string idempotencyKey)` in an in-memory service.
> - Validate amount > 0, no self-transfer, wallets exist, enough balance.
> - Same key again → return the original result.
> - It must be safe when called from many threads at once.

**Starter code**

```csharp
public record TransferResult(bool Success, string? Reason, decimal? BalanceAfter);

public class WalletService
{
    private readonly Dictionary<string, decimal> _wallets = new() { ["A"] = 100m, ["B"] = 50m };
    // your code
}
```

**Answer key**

```csharp
private readonly ConcurrentDictionary<string, TransferResult> _processed = new();
private readonly object _lock = new();

public TransferResult Transfer(string from, string to, decimal amount, string key)
{
    if (_processed.TryGetValue(key, out var existing)) return existing;
    lock (_lock)
    {
        if (_processed.TryGetValue(key, out existing)) return existing; // double-check inside the lock
        TransferResult result;
        if (amount <= 0) result = new(false, "INVALID_AMOUNT", null);
        else if (from == to) result = new(false, "SAME_WALLET", null);
        else if (!_wallets.ContainsKey(from) || !_wallets.ContainsKey(to)) result = new(false, "UNKNOWN_WALLET", null);
        else if (_wallets[from] < amount) result = new(false, "INSUFFICIENT_BALANCE", null);
        else
        {
            _wallets[from] -= amount;
            _wallets[to] += amount;
            result = new(true, null, _wallets[from]);
        }
        _processed[key] = result;
        return result;
    }
}
```

**Follow-ups**

- Why `decimal` and not `double`?
- One global lock is simple but slow. Alternatives? (Per-wallet locks with ordered locking to avoid deadlocks.)
- How does this look with SQL Server? (Unique index on the key; `UPDATE Wallets SET Balance = Balance - @amt WHERE Id = @from AND Balance >= @amt` inside a transaction; check rows affected.)

---

## NET-L2: Code review of a controller

**Time:** 15 min · **Type:** code review

**Prompt**

> Review this controller before it goes to production. List the problems and the fixes.

**Starter code**

```csharp
[ApiController, Route("api/payments")]
public class PaymentsController : ControllerBase
{
    private static AppDbContext _db = new AppDbContext();

    [HttpGet("{merchantId}")]
    public IActionResult Get(string merchantId)
    {
        var sql = "SELECT * FROM Payments WHERE MerchantId = '" + merchantId + "'";
        var rows = _db.Payments.FromSqlRaw(sql).ToList();
        return Ok(rows);
    }

    [HttpPost("refund")]
    public IActionResult Refund(int paymentId, double amount)
    {
        var p = _db.Payments.FindAsync(paymentId).Result;
        p.RefundedAmount += amount;
        _db.SaveChanges();
        return Ok(p);
    }
}
```

**Answer key (look for at least 6)**

1. Static `DbContext` shared by all requests: not thread-safe, stale data, memory growth. Inject it as Scoped.
2. **SQL injection** in the string-built query. Use LINQ or parameters (`FromSqlInterpolated`).
3. No `[Authorize]`: anyone can read any merchant's payments or refund them.
4. `.Result` blocks a thread and can deadlock. Use `async`/`await`.
5. `double` for money: use `decimal`.
6. No validation: payment may be null (crash); amount ≤ 0; refund over the payment amount; payment not `SUCCESS`.
7. No concurrency control: two refunds at once can exceed the limit. Use a row version or a conditional update.
8. Returns the entity directly (over-exposes fields). Use a DTO.
9. `SELECT *` and no paging on the GET.
10. No audit log, no idempotency on refund.

---

## NET-L3: LINQ and SQL

**Time:** 10–15 min · **Type:** query writing

**Prompt**

> Table `Transactions(Id, UserId, Amount, Type, Status, CreatedAt)` where `Type` is `CREDIT` or `DEBIT`.
> 1. Write a **SQL** query for each user's net amount (credits minus debits) of `SUCCESS` transactions in October 2026.
> 2. Write the same in **LINQ** with EF Core.
> 3. Write SQL to find transactions with the same `UserId` and `Amount` created within 60 seconds of each other.

**Answer key**

```sql
-- 1
SELECT UserId,
       SUM(CASE WHEN Type = 'CREDIT' THEN Amount ELSE -Amount END) AS Net
FROM Transactions
WHERE Status = 'SUCCESS' AND CreatedAt >= '2026-10-01' AND CreatedAt < '2026-11-01'
GROUP BY UserId;

-- 3
SELECT a.Id, b.Id AS DuplicateId
FROM Transactions a
JOIN Transactions b
  ON a.UserId = b.UserId AND a.Amount = b.Amount AND a.Id < b.Id
 AND b.CreatedAt BETWEEN a.CreatedAt AND DATEADD(SECOND, 60, a.CreatedAt);
```

```csharp
// 2
var start = new DateTime(2026, 10, 1); var end = start.AddMonths(1);
var net = await db.Transactions
    .Where(t => t.Status == "SUCCESS" && t.CreatedAt >= start && t.CreatedAt < end)
    .GroupBy(t => t.UserId)
    .Select(g => new { UserId = g.Key, Net = g.Sum(t => t.Type == "CREDIT" ? t.Amount : -t.Amount) })
    .ToListAsync();
```

**Follow-ups:** why `< '2026-11-01'` instead of `<= '2026-10-31'`; which index helps (`Status, CreatedAt` including `UserId, Amount, Type`); `AsNoTracking()` for reads.

---

## NET-L4: Middleware and DI questions

**Time:** 10 min · **Type:** discussion

1. "Write a middleware that adds an `X-Correlation-Id` header to every response (reuse it if the request already has one) and puts it in the log scope."
   → `app.Use(async (ctx, next) => { var id = ctx.Request.Headers["X-Correlation-Id"].FirstOrDefault() ?? Guid.NewGuid().ToString(); ctx.Response.Headers["X-Correlation-Id"] = id; using (logger.BeginScope(new { CorrelationId = id })) await next(); });`
2. "Where does it go in the pipeline relative to authentication and exception handling?" → Early: before the exception handler logs, so errors carry the ID.
3. "A Singleton service needs the `DbContext`. How do you do it safely?" → Inject `IServiceScopeFactory` and create a scope per operation.
