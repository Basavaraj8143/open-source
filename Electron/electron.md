#  Open Source Contribution — Electron Documentation (Localization)

![Open Source](https://img.shields.io/badge/Open%20Source-Contribution-brightgreen?style=flat-square)
![Platform](https://img.shields.io/badge/Platform-Electron-47848F?style=flat-square&logo=electron)
![Area](https://img.shields.io/badge/Area-Documentation%20%7C%20Localization-blue?style=flat-square)
![Status](https://img.shields.io/badge/Status-Resolved-success?style=flat-square)

---

## 📌 Project

| Field | Details |
|---|---|
| **Repository** | [Electron](https://github.com/electron/electron) |
| **Area** | Documentation — Chinese Localization via Crowdin |
| **File Affected** | `menu-item.md` (Chinese Simplified) |

---

## 🎯 Problem Identified

The issue involved **incorrect translation of `MenuItem` enumeration values** in the Chinese API documentation.

- Enum values like `normal`, `checkbox`, `radio`, etc. were being translated into Chinese — these are **code-level constants** that must remain in English as-is.
- Additional issues included:
  - ❌ Incorrect HTML `<code>` tag formatting
  - ❌ Extra spaces inside code tags (e.g., `<code> type </code>`)
  - ❌ Punctuation inconsistencies (`.` used where `。` was required)
  - ❌ Broken HTML links and mismatched variables
  - ❌ Duplicate lines and unresolved variable warnings

---

## 🛠️ Work Done

### 1. 🔍 Investigated the Issue
- Analyzed the problem reported in the GitHub issue thread.
- Identified that Electron uses **Crowdin** for translation management — not direct GitHub PRs.

### 2. ✅ Followed Correct Workflow
- Used the **Crowdin platform** to locate and fix the affected strings.
- Avoided a GitHub PR, respecting the project's localization workflow.

### 3. 🔧 Fixed Translation Errors

| Error Type | Before | After |
|---|---|---|
| Enum value translated | `复选框` | `checkbox` |
| Extra spaces in code tag | `<code> type </code>` | `<code>type</code>` |
| Wrong punctuation | `.` | `。` |
| Broken links | Mismatched HTML | Copied exact source values |

- Ensured `checkbox`, `radio`, `normal`, and similar enum values were **never translated**
- Corrected all `<code>` block formatting issues
- Removed duplicate lines and resolved all missing variable warnings

### 4. 🧪 Validation
- Cleared **all Crowdin validation errors** (red warnings)
- Verified consistency with Electron's localization guidelines

### 5. 🤝 Collaboration
- Communicated progress directly on the GitHub issue
- Maintainers reviewed and applied final adjustments
- Issue was successfully marked as **resolved** ✅

---

## 📸 Screenshots

![Screenshot 1](./Screenshot%202026-04-15%20200734.png)
![Screenshot 2](./Screenshot%202026-04-15%20200744.png)
![Screenshot 3](./Screenshot%202026-04-15%20200756.png)
![Screenshot 4](./Screenshot%202026-04-15%20200805.png)

---

## 🧠 Key Learnings

> *Open source is more than writing code.*

- 📝 **Documentation and localization** are equally critical contributions
- 🔄 Always respect **project-specific workflows** (Crowdin, not PRs, in this case)
- 🔍 Small issues — formatting, punctuation, consistency — have real-world impact on developer experience
- 🤝 **Maintainer collaboration** is a core part of the contribution process
- 🎯 Precision matters, especially in structured formats like API documentation

---

## ✅ Outcome

- ✔️ Contributed to improving Electron's **Chinese Simplified API documentation**
- ✔️ Resolved inconsistencies in `MenuItem` API representation
- ✔️ Successfully participated in a **real-world open source workflow**
- ✔️ Issue closed and accepted by Electron maintainers

---

## 🔗 References

- 🐛 [Electron GitHub Issue — Localization Fix](https://github.com/electron/electron)
- 🌐 [Crowdin Translation Platform](https://crowdin.com/project/electron)
- 📄 [Electron Localization Guide](https://github.com/electron/i18n)

---

<div align="center">

*Made with ❤️ as part of open source learning — contributions aren't just code.*

</div>
