# Live Coding Round (on the interview call)

Short tasks the candidate solves **live on the call** while sharing their screen. Use these when you do not want to send a take-home assessment, or as a quick check after one.

> **Internal only.** Each file contains answer keys. Paste only the **Prompt** (and **Starter code**, if any) into the call chat.

## Format (45–60 min)

| Time | What happens |
| --- | --- |
| 5 min | Introductions; the candidate opens their own editor and shares their screen |
| 5 min | Warm-up task (all developer roles: [W-01](#w-01-split-money-without-losing-a-paisa) below) |
| 15–25 min | Main task 1 |
| 15–20 min | Main task 2 (a code review or a second small task) |
| 5–10 min | Follow-up questions and the candidate's questions |

**Rules to tell the candidate:**

- Use your own editor and language setup. Googling syntax is fine.
- **No AI assistants** during this round. (This round checks how you think, not how fast you can prompt.)
- Think out loud. A partial solution with clear reasoning scores better than a silent full one.

**Pick 2 tasks per candidate:** one coding task plus one code-review task works best.

## Tasks by role

| Role | File | Tasks |
| --- | --- | --- |
| Full Stack (MERN) | [full-stack-mern.md](full-stack-mern.md) | FS-L1 to FS-L4 |
| Backend (Node.js) | [backend-nodejs.md](backend-nodejs.md) | BE-L1 to BE-L4 |
| Frontend (React / Next.js) | [frontend-react-nextjs.md](frontend-react-nextjs.md) | FE-L1 to FE-L4 |
| .NET Developer | [dotnet-developer.md](dotnet-developer.md) | NET-L1 to NET-L4 |
| QA Engineer | [qa-engineer.md](qa-engineer.md) | QA-L1 to QA-L4 |
| Flutter Developer | [flutter-developer.md](flutter-developer.md) | FL-L1 to FL-L4 |
| Android Developer (Kotlin) | [android-kotlin.md](android-kotlin.md) | AND-L1 to AND-L4 |

## Scoring (1–4 per task)

| Score | Meaning |
| --- | --- |
| 4 | Working solution, handles edge cases without prompting, explains trade-offs |
| 3 | Working solution, needs a hint for one or two edge cases |
| 2 | Partial solution; understands the problem but struggles to implement it |
| 1 | Cannot get started, or the approach is wrong |

Also score **communication** (thinking aloud, asking clarifying questions) from 1 to 4.

---

## W-01: Split money without losing a paisa

**Time:** 5–10 min · **Roles:** all developer roles (any language)

**Prompt**

> Write a function that splits an amount in rupees between N people as evenly as possible. Work in paise. The parts must add up exactly to the original amount.
> Example: `split(100, 3)` → `[33.34, 33.33, 33.33]`

**Answer key**

- Convert to integer paise first: `total = Math.round(amount * 100)`.
- `base = Math.floor(total / n)`, `remainder = total % n`; the first `remainder` people get `base + 1`.
- Convert back only for display.

```js
function split(amount, n) {
  const total = Math.round(amount * 100);
  const base = Math.floor(total / n), rem = total % n;
  return Array.from({ length: n }, (_, i) => (base + (i < rem ? 1 : 0)) / 100);
}
```

**Follow-ups**

- Why not just `amount / n` rounded to 2 decimals? (The parts may not add up: 33.33 × 3 = 99.99.)
- What if `n` is 0 or negative, or the amount is negative?
- How would you split by percentages (70 / 30) instead?

**Red flag:** uses floating point throughout and doesn't check the sum.
