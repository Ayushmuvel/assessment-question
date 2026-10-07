# Frontend (React / Next.js): Live Round Tasks

Paste only the **Prompt** and **Starter code** into the call chat. Answer keys are for the interviewer.
FS-L3 (Pay button) from [full-stack-mern.md](full-stack-mern.md) also works well for frontend candidates.

---

## FE-L1: `useDebounce` hook with search

**Time:** 15 min · **Type:** coding (React + TypeScript)

**Prompt**

> Write a `useDebounce(value, delayMs)` hook. Use it in a search box so the API `searchTransactions(query)` is called only after the user stops typing for 400 ms. Show the results in a list.

**Answer key**

```tsx
function useDebounce<T>(value: T, delay: number): T {
  const [debounced, setDebounced] = useState(value);
  useEffect(() => {
    const id = setTimeout(() => setDebounced(value), delay);
    return () => clearTimeout(id);
  }, [value, delay]);
  return debounced;
}

function Search() {
  const [q, setQ] = useState("");
  const dq = useDebounce(q, 400);
  const [rows, setRows] = useState<Txn[]>([]);
  useEffect(() => {
    if (!dq) return setRows([]);
    let active = true;
    searchTransactions(dq).then(r => { if (active) setRows(r); });
    return () => { active = false; };
  }, [dq]);
  return (<><input value={q} onChange={e => setQ(e.target.value)} aria-label="Search" />
          <ul>{rows.map(r => <li key={r.id}>{r.id}</li>)}</ul></>);
}
```

**Follow-ups**

- Why the cleanup in the debounce effect?
- Why the `active` flag? (Stops an older, slower response overwriting a newer one.) Alternative: `AbortController`.
- How would you keep the query in the URL in Next.js? (`useSearchParams` + `router.replace`.)

---

## FE-L2: Fix the buggy component

**Time:** 15 min · **Type:** code review / debugging

**Prompt**

> This component shows a merchant's transactions. Users report: wrong data sometimes shows after switching merchants quickly, the console shows warnings, and the list flickers. Find and fix the bugs.

**Starter code**

```jsx
function Transactions({ merchantId }) {
  const [data, setData] = useState([]);
  const [page, setPage] = useState(1);

  useEffect(async () => {
    const res = await fetch(`/api/txns?merchant=${merchantId}&page=${page}`);
    setData(await res.json());
  });

  return (
    <div>
      {data.map((t, i) => <Row key={i} txn={t} onClick={() => setPage(page + 1)} />)}
      <p>Total: ₹{data.reduce((s, t) => s + t.amount, 0)}</p>
    </div>
  );
}
```

**Answer key**

1. `useEffect` callback is `async`: it returns a promise, not a cleanup. Define an inner async function.
2. No dependency array: runs after **every** render → infinite fetch loop. Use `[merchantId, page]`.
3. Race condition: a slow old response overwrites the new merchant's data. Use `AbortController` or an `active` flag.
4. `page` doesn't reset when `merchantId` changes.
5. `key={i}`: use `t.id`.
6. No loading or error state; `res.ok` not checked.
7. Amounts summed as numbers. If they are rupees as floats, display may be wrong (use paise + formatting with `Intl.NumberFormat("en-IN", { style: "currency", currency: "INR" })`).
8. Paging by clicking a row is odd UX (bonus discussion).

**Scoring:** finds 1–3 and fixes them correctly = 3; also 4–6 = 4.

---

## FE-L3: Amount input and card masking

**Time:** 15 min · **Type:** coding

**Prompt**

> Build an `AmountInput` that:
> - accepts only numbers with up to 2 decimals,
> - shows the value formatted as Indian currency when not focused (₹1,25,000.50),
> - calls `onChange` with the value in **paise** (integer).
> Also write `maskCard("4111111111111111")` → `"•••• •••• •••• 1111"`.

**Answer key points**

- Regex on input: `/^\d*(\.\d{0,2})?$/`.
- Formatting: `new Intl.NumberFormat("en-IN", { style: "currency", currency: "INR" }).format(n)`.
- Convert to paise with `Math.round(parseFloat(v) * 100)`; never store floats.
- Keep the raw text in state while focused; format on blur.
- `maskCard`: `"•••• •••• •••• " + num.slice(-4)`; handle short input.
- **Follow-up:** accessibility. Label, `inputMode="decimal"`, and error text linked with `aria-describedby`.

---

## FE-L4: Next.js rendering questions with code

**Time:** 10–15 min · **Type:** discussion

**Prompt (ask one at a time)**

1. "Here's a page that shows public exchange rates updated every 10 minutes. How do you render it in the App Router?"
   → Server Component with `fetch(url, { next: { revalidate: 600 } })` (ISR).
2. "The merchant dashboard needs the logged-in user's data. Where do you fetch it, and how do you protect the route?"
   → Server Component reading the session from cookies, or middleware redirect; never cache per-user data publicly.
3. "This component uses `useState` and `window.localStorage` and throws a hydration error. Why, and how do you fix it?"
   → Mark it `'use client'`; read `localStorage` inside `useEffect`, not during render.
4. "How do you show a loading skeleton while the dashboard data loads?"
   → `loading.tsx` or `<Suspense fallback>` around the async component.
