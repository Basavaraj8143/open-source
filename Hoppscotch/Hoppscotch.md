# 🧑‍💻 Open Source Contribution — Hoppscotch

![Open Source](https://img.shields.io/badge/Open%20Source-Contribution-brightgreen?style=flat-square)
![Project](https://img.shields.io/badge/Project-Hoppscotch-FF6B35?style=flat-square)
![Area](https://img.shields.io/badge/Area-CLI%20Module%20%7C%20Logic%20Bug-blue?style=flat-square)
![PR Status](https://img.shields.io/badge/PR%20%236122-Merged-success?style=flat-square)

---

## 📌 Project

[Hoppscotch](https://github.com/hoppscotch/hoppscotch) — an open-source API development ecosystem used by developers worldwide.

---

## 🐞 Issue Details

| Field | Details |
|---|---|
| **Issue Number** | [#6118](https://github.com/hoppscotch/hoppscotch/issues/6118) |
| **Title** | `hasFailedTestCases` returns `true` when all tests pass, `false` when tests fail |
| **Type** | Logic Bug — CLI Module |

---

## ❗ Problem Statement

The function `hasFailedTestCases` had **inverted logic**:

- ✅ Returned `true` when **all tests passed**
- ❌ Returned `false` when **any test failed**

This directly contradicted both the function name and its own JSDoc:

> *"True, if one or more failed test-cases found"*

---

## 🔍 Root Cause

The implementation used:

```ts
A.every(({ failed }) => failed === 0)
```

| Method | Behavior |
|---|---|
| `every()` | Returns `true` only if **all** elements satisfy the condition |
| `failed === 0` | Means "no failures" |

Combined, this returned `true` **only when everything passed** — the exact opposite of what the function name implied.

---

## 🛠️ My Approach

I analyzed the logic and proposed replacing it with:

```ts
// Before (incorrect)
A.every(({ failed }) => failed === 0)

// Proposed fix
A.some(({ failed }) => failed > 0)
```

**Why `some()` + `failed > 0`:**
- `some()` returns `true` if **at least one** element satisfies the condition
- `failed > 0` detects the presence of a failure

This aligned with the function name, the JSDoc description, and the expected behavior.

---

## ⚠️ Review Feedback & Insight

During PR review, the maintainers identified a key constraint:

- The function was also used in `request.ts`
- That usage depended on the **original (incorrect) behavior**
- Changing the logic alone would silently break that call site

This made a pure logic swap risky without updating all consumers.

---

## ✅ Final Solution — Merged

The maintainer proposed a cleaner design fix:

### ① Rename the function to match the existing logic

```ts
// Before
hasFailedTestCases

// After
hasAllTestsPassed
```

### ② Keep the original logic intact

```ts
A.every(({ failed }) => failed === 0)  // ✅ now correctly named
```

### ③ Update the call site in `request.ts`

```ts
report.result = report.result && _allTestsPassed;
```

> Renaming made the code honest without touching behavior — a minimal, safe, and semantically correct fix.

---

## 🎯 Why This Solution Was Better

| Approach | Risk | Clarity |
|---|---|---|
| Change logic (`some` + `failed > 0`) | Medium — multiple call sites affected | Good |
| Rename function (`hasAllTestsPassed`) | Low — no behavioral change | ✅ Better |

- ✔️ No risk of breaking existing behavior
- ✔️ Minimal system-wide impact
- ✔️ Naming now accurately reflects the implementation
- ✔️ Consistent across the entire codebase

---

## 📦 Pull Request

| Field | Details |
|---|---|
| **PR Title** | `refactor(cli): match test-result helper name to documented contract` |
| **PR Number** | [#6122](https://github.com/hoppscotch/hoppscotch/pull/6122) |
| **Status** | ✅ Merged |

---

## 📸 Screenshots

![Screenshot 1](./Screenshot%202026-04-15%20200411.png)
![Screenshot 2](./Screenshot%202026-04-15%20200429.png)
![Screenshot 3](./Screenshot%202026-04-15%20200436.png)

---

## 🧠 Key Learnings

> *A function's name is a contract. When code breaks that contract, either fix the code — or fix the name.*

- 🔎 Logic must align with naming **and** documentation
- 🔗 Always trace how a function is used before modifying it
- 🪶 Minimal, low-risk changes are preferred in production codebases
- 💡 Multiple valid solutions can exist — the best one minimizes impact
- 🤝 Code reviews surface blind spots and lead to better design decisions

---

## 🚀 Contribution Summary

- ✔️ Identified and analyzed a logic inconsistency in the CLI module
- ✔️ Proposed a fix and opened a pull request
- ✔️ Collaborated with maintainers through the review process
- ✔️ Adapted to an improved solution (function renaming over logic change)
- ✔️ PR merged into production — contributing to a widely-used open-source tool

---

## 🔗 References

- 🐛 [Issue #6118 — hasFailedTestCases logic bug](https://github.com/hoppscotch/hoppscotch/issues/6118)
- 🔀 [PR #6122 — Merged fix](https://github.com/hoppscotch/hoppscotch/pull/6122)
- 🌐 [Hoppscotch Repository](https://github.com/hoppscotch/hoppscotch)

---

<div align="center">

*Sometimes the best fix isn't changing what code does — it's changing what you call it.*

</div>
