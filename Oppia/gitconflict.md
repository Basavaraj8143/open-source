# 🧑‍💻 Open Source Contribution — Oppia
## Resolving Merge Conflicts After PR Approval (Without Force Push)

![Open Source](https://img.shields.io/badge/Open%20Source-Contribution-brightgreen?style=flat-square)
![Project](https://img.shields.io/badge/Project-Oppia-blue?style=flat-square)
![Git](https://img.shields.io/badge/Git-Merge%20Conflict-orange?style=flat-square)

---

## 📌 Context

While working on **Oppia Issue #26108** (Cleanup remaining allowlisted files for Angular strict template type-checking), my PR had already reached the final review stage:

- ✅ All CI checks passed
- ✅ Two code-owner approvals
- ✅ `PR: LGTM` label added
- ✅ Auto-merge enabled

Then, before GitHub could merge it, new commits landed on `develop` — and GitHub flagged:

```
PR: don't merge - HAS MERGE CONFLICTS
```

---

## What Happened

Once `develop` moved ahead of my approved branch, GitHub could no longer auto-merge it.

```
PR Created
      │
      ▼
CI Passed
      │
      ▼
2 Code Owner Approvals
      │
      ▼
Auto Merge Enabled
      │
      ▼
New commits merged into develop
      │
      ▼
Merge Conflict
```

---

## Previous Mistake (Earlier Attempt)

Earlier in the contribution, I tried resolving conflicts by rebasing onto the latest `develop`:

```bash
git fetch upstream
git checkout strict-checks-blog-dashboard-v5
git rebase upstream/develop
```

Since the history changed, Git demanded a force push:

```bash
git push --force-with-lease
```

The maintainers closed the PR — Oppia discourages rewriting history once a PR is in review. On top of that, resolving conflicts manually during the rebase introduced unrelated changes into `scripts/template_strict_exclude_paths.txt`, which reviewers flagged immediately.

**Lesson learned:**
- ❌ Don't rewrite commit history after a reviewed PR.
- ❌ Avoid force-pushing once reviewers have started reviewing your work.

---

## Final Approach — Merge Instead of Rebase

To preserve the existing review history, I resolved the conflicts through a merge commit instead.

### Step 1 — Fetch latest changes
```bash
git fetch upstream
```

### Step 2 — Create a temporary merge branch
```bash
git checkout -b strict-checks-blog-dashboard-v5-merge
```
This keeps the original PR branch untouched.

### Step 3 — Merge latest develop
```bash
git merge upstream/develop
```
Git reported conflicts in:
```
core/templates/pages/admin-page/platform-parameters-tab/admin-platform-parameters-tab.component.ts
core/templates/pages/blog-dashboard-page/blog-post-editor/blog-post-editor.component.ts
```

While resolving, I:
- kept the incoming upstream changes
- preserved my strict-template fixes
- removed accidental conflict artifacts
- double-checked that only intended changes remained

### Step 4 — Verify locally

Run strict template checks:
```bash
python -m scripts.run_typescript_checks --strict_checks
```
```
Angular template compilation successful!
```

Run frontend tests:
```bash
python -m scripts.run_frontend_tests \
  --run_on_changed_files_in_branch \
  --check_coverage
```
All passed.

### Step 5 — Push without force
```bash
git push origin strict-checks-blog-dashboard-v5-merge:strict-checks-blog-dashboard-v5
```
No `--force`, no history rewrite — the existing PR was updated normally.

---

## GitHub Timeline After Push

Immediately after pushing, GitHub disabled auto-merge:
```
Head branch was pushed to by a user without write access.
```
This is expected behavior whenever new commits are pushed. A few minutes later:

- ✅ Merge conflict label removed
- ✅ CI restarted automatically
- ✅ Previous approvals remained intact
- ✅ Maintainer re-enabled auto-merge

```
Merge Conflict
      │
      ▼
Create Merge Commit
      │
      ▼
Normal Push
      │
      ▼
Auto Merge Disabled
      │
      ▼
Conflict Label Removed
      │
      ▼
CI Restarted
      │
      ▼
Auto Merge Enabled Again
```

---

## Useful Git Commands

| Purpose | Command |
|---|---|
| Check current status | `git status` |
| View recent commits | `git log --oneline --graph --decorate -10` |
| Compare against upstream | `git diff upstream/develop...HEAD` |
| List changed files | `git diff --name-only upstream/develop...HEAD` |
| Verify only intended files changed | `git diff upstream/develop -- scripts/template_strict_exclude_paths.txt` |
| Push merge branch into PR branch | `git push origin strict-checks-blog-dashboard-v5-merge:strict-checks-blog-dashboard-v5` |

---

## Key Learnings

**❌ Avoid**
- Force-pushing after review has started
- Rebasing a reviewed PR unless maintainers explicitly request it
- Resolving conflicts without checking the final diff
- Accidentally introducing unrelated changes

**✅ Prefer**
- Preserving review history
- Resolving conflicts locally, deliberately
- Using a merge commit for already-reviewed PRs
- Running local checks before pushing
- Inspecting the final diff before updating the PR

---

## Outcome

- ✅ Merge conflicts resolved successfully
- ✅ No force push required
- ✅ Existing approvals preserved
- ✅ Conflict label removed
- ✅ CI reran automatically
- ✅ Auto-merge re-enabled by maintainer

---

## Final Takeaway

Maintaining **review history** matters just as much as maintaining a clean **commit history** in large open-source projects. When a reviewed PR develops merge conflicts, resolving them with a merge commit — rather than rewriting history — preserves approvals and sidesteps the fallout of force-pushing. The exact workflow varies by project, but understanding and following each project's contribution guidelines is what actually matters.
