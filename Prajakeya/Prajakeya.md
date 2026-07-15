# 🧑‍💻 Open Source Contribution — Prajaakeeya Backend

![Open Source](https://img.shields.io/badge/Open%20Source-Contribution-brightgreen?style=flat-square)
![Project](https://img.shields.io/badge/Project-Prajaakeeya-6C63FF?style=flat-square)
![Area](https://img.shields.io/badge/Area-Business%20Logic-orange?style=flat-square)
![Status](https://img.shields.io/badge/PR-Open-yellow?style=flat-square)

---

# 📌 Project

**Prajaakeeya Backend** is a NestJS-based backend powering a civic engagement platform that enables voters to interact with election aspirants, participate in meetings, raise local issues, and vote during election windows.

Repository:

https://github.com/prajaakeeya/prajaakeeya-backend

---

# 🐞 Issue Details

| Field            | Details                                                  |
| ---------------- | -------------------------------------------------------- |
| **Issue Number** | #31                                                      |
| **Title**        | Voting eligibility check ignores Direct Meet interaction |
| **Type**         | Business Logic Bug                                       |

---

# ❗ Problem Statement

The voting flow requires users to interact with an aspirant before becoming eligible to vote.

Supported interactions include:

* Chat
* Meeting
* Direct Meet
* Phone Call

However, users who interacted **only through a Direct Meet** were still considered ineligible.

---

# 🔍 Root Cause

Voting eligibility relies on:

```ts
UsersService.hasAnyInteraction()
```

The implementation checked:

```ts
return (
    user.isChat ||
    user.isMeeting ||
    user.isPhoneCall
);
```

Although the application tracks:

```ts
isDirectMeet
```

it was omitted from the eligibility validation.

As a result:

```
Direct Meet completed
        ↓
isDirectMeet = true
        ↓
hasAnyInteraction()
        ↓
returns false
        ↓
Voting rejected
```

---

# 🛠️ My Approach

I analyzed the interaction flow and verified that:

* `isDirectMeet` exists in the `User` entity.
* Direct Meet interactions are mapped correctly by the interaction tracking logic.
* The eligibility helper did not consider `isDirectMeet`.

To resolve the inconsistency, I updated the helper to include the missing interaction type.

```ts
return (
    user.isChat ||
    user.isMeeting ||
    user.isPhoneCall ||
    user.isDirectMeet
);
```

---

# ✅ Regression Tests Added

To ensure the behavior remains correct, I added unit tests covering:

* User not found
* User with no interactions
* User with **only Direct Meet** interaction
* Existing interaction types

---

# ✔ Verification

Local validation completed successfully.

```
Test Suites: 37 passed
Tests: 254 passed
Snapshots: 0
```

---

# 💬 Maintainer Feedback

After opening the PR, the maintainer responded that the omission of `isDirectMeet` was **intentional**, based on previous client feedback.

> "We had actually omitted this purposefully based on client's feedback. I'll discuss with the team regarding this again and if needed we'll accept your valuable work."

This highlighted an important lesson:

> Not every apparent bug is a coding mistake—sometimes implementation reflects business decisions that aren't obvious from the code alone.

The discussion is currently ongoing while the team re-evaluates the requirement.

---

# 📦 Pull Request

| Field         | Details                                                     |
| ------------- | ----------------------------------------------------------- |
| **PR Number** | #32                                                         |
| **Title**     | `fix: include direct meet interaction in hasAnyInteraction` |
| **Status**    | 🟡 Open                                                     |

---

# 📸 Screenshots

* Issue opened
* Pull Request created
* All tests passing
* Maintainer discussion

---

# 🧠 Key Learnings

* Business logic bugs require understanding both the code and the intended product behavior.
* Reading the complete interaction flow is essential before proposing a fix.
* Regression tests strengthen even small fixes.
* Open-source review discussions often reveal hidden business requirements that are not documented in the code.
* Even when a change is technically correct, maintainers may need to validate it against product requirements before merging.

---

# 🚀 Contribution Summary

* ✔ Identified a business logic inconsistency in voting eligibility.
* ✔ Traced the interaction flow to determine the root cause.
* ✔ Implemented the fix.
* ✔ Added regression tests.
* ✔ Verified all existing tests pass.
* ✔ Opened an issue and submitted a pull request.
* ✔ Participated in technical discussion with maintainers regarding business requirements.

---

# 🔗 References

* Issue #31
* PR #32
* https://github.com/prajaakeeya/prajaakeeya-backend

---

<div align="center">

*"Correct code is only one part of software engineering; understanding business requirements is equally important."*

</div>
