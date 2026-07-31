# 🧑‍💻 Open Source Contribution — Oppia

This directory contains documentation and work logs related to my open source contribution to [Oppia](https://github.com/oppia/oppia).

## 📌 Contribution Details

- **Repository:** https://github.com/oppia/oppia
- **Issue:** #26108 - Cleanup remaining allowlisted files for Angular strict template type-checking (Group 5)
- **Pull Requests:** #26275 (Initial attempt) / #26866 (Final merged PR)
- **Status:** ✅ Merged

## 📁 Directory Contents

This folder contains three detailed markdown files documenting different stages and challenges of the contribution process:

### 1. [`Oppia.md`](./Oppia.md)
**Oppia Contribution Work Log**
A comprehensive log of the initial task, development work (Angular strict template fixes), reviewer feedback, and the overall workflow. It covers the mistakes made initially (like force-pushing and modifying project-wide `.gitignore`), and how they were corrected by creating a fresh branch.

### 2. [`gitconflict.md`](./gitconflict.md)
**Resolving Merge Conflicts After PR Approval (Without Force Push)**
A detailed guide on how merge conflicts were resolved after the PR had already received approvals and was ready for auto-merge. It explains why rebasing and force-pushing were rejected, and how merging `upstream/develop` into the feature branch preserved the review history and approvals.

### 3. [`finally.md`](./finally.md)
**Post Merge-Conflict Timeline & CI Debugging**
Documents the events following the merge conflict resolution. It focuses heavily on debugging continuous integration (CI) failures, specifically differentiating between cleanup errors (like Elasticsearch or Redis shutting down) and the actual root cause (a flaky Puppeteer acceptance test where a SAVE button timed out). 

## 📚 Key Learnings & Takeaways

Across these documents, several important lessons about large-scale open-source collaboration were documented:
- **Git & GitHub Workflow:** Avoid force-pushing after reviews have started to preserve review history. Use a merge commit rather than rebasing to resolve conflicts on approved PRs.
- **Oppia's Processes:** Follow specific maintainer instructions (e.g., using `@username PTAL` rather than manually assigning reviewers), run local typescript and coverage checks before pushing, and leave project-wide `.gitignore` files untouched.
- **CI & Debugging:** When GitHub Actions fail, read the logs carefully. Passing unit tests do not guarantee passing coverage checks. Understand how to distinguish test cleanup error logs from the actual test failure, and search existing repository issues for known flaky tests before reporting new ones.
