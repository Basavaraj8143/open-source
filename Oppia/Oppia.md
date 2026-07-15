# Oppia Contribution Work Log

## Repository
- Project: Oppia
- Issue: #26108
- Task: Cleanup remaining allowlisted files for Angular strict template type-checking (Group 5)

---

# Initial Task

Assigned to remove the Group 5 files from:

scripts/template_strict_exclude_paths.txt

and make all corresponding Angular strict template type-checking fixes so these files no longer require the allowlist.

Affected files included:

- admin-platform-parameters-tab.component.ts
- android-page.component.ts
- blog-admin-page.component.ts
- blog-author-profile-page.component.ts
- blog-dashboard-tile.component.ts
- blog-post-editor.component.ts
- upload-blog-post-thumbnail.component.ts
- blog-dashboard-navbar-breadcrumb.component.ts
- blog-post-editor-pre-logo-action.component.ts
- blog-home-page.component.ts

---

# Development Work

Implemented all required strict template fixes.

Main changes included:

- Improved TypeScript typings.
- Added proper optional chaining and null safety.
- Fixed template input type mismatches.
- Replaced template access to private services with public wrapper methods.
- Updated schema typings to shared interfaces.
- Fixed boolean template bindings.
- Added missing template-bound properties.
- Updated tests where required.

---

# Local Verification

Executed:

```bash
python -m scripts.run_typescript_checks --strict_checks
```

Result:

- TypeScript compilation successful.
- Angular template compilation successful.
- Strict template checks passed locally.

Captured screenshot as proof.

---

# Initial PR

Created PR:

Fix part of #26108: Fix strict template checks for blog dashboard files

Included:

- Overview
- Checklist
- Detailed explanation
- Verification screenshot

---

# First Review Feedback

Reviewer explained the expected workflow.

Problem:

I opened the PR before:

- Sharing local strict-check screenshot.
- Being officially assigned.

Reviewer clarified that:

Future contributors should first:

1. Run strict checks locally.
2. Upload screenshot in issue thread.
3. Wait for assignment.
4. Only then create the PR.

PR itself was allowed to remain open.

---

# Reviewer Assignment Requirement

Oppia bot requested reviewer assignment.

Initially I did not know contributors cannot directly assign reviewers.

Maintainer instructed me to comment:

@username PTAL

instead of assigning reviewers manually.

---

# .gitignore Review

Reviewer noticed:

.gitignore

contained:

+.venv/

Review comment:

Project-wide shared files should not be modified for local environment setup.

Instruction:

Use global gitignore instead.

---

# First Mistake

Instead of creating a clean commit removing the .gitignore change, I:

Force pushed rewritten history.

This caused the maintainer to close the PR.

Maintainer comment:

Avoid force-pushing rewritten history.

Instead:

- Create a fresh branch.
- Cherry-pick only required commits.
- Open a new PR.

---

# Creating New Branch

Started again.

Created:

strict-checks-blog-dashboard-v3

based on latest develop.

Cherry-picked only:

Fix strict template checks for blog dashboard files

Verified that:

.gitignore change was no longer included.

Checked with:

```bash
git show --stat HEAD
```

Only expected files remained.

---

# New PR

Opened a fresh PR.

All previous review comments resolved.

Workflow followed correctly.

---

# Reviewer Feedback

Reviewer requested:

Variable:

last

was too generic.

Requested better naming.

---

# Code Change

Renamed:

last

to:

isLastBlogPostInList

Updated:

blog-dashboard-page.component.html

blog-dashboard-tile.component.ts

blog-dashboard-tile.component.html

Verified no other references existed.

Committed changes.

---

# Stale PR

Due to no reviewer activity for several days:

Oppia bot marked PR as stale.

Commented:

Addressed the review comments and pushed the requested changes PTAL!

Bot removed stale label.

Later stale occurred again.

Left follow-up reminder.

Maintainer re-triggered review.

---

# CI Failure

All template checks passed.

However GitHub Actions failed.

Failure:

Frontend Coverage Checks Not Passed

Tests themselves succeeded.

Failure occurred only during coverage validation.

---

# Investigation

Downloaded coverage artifacts.

Coverage report showed uncovered code only in:

- blog-post-editor.component.ts
- blog-dashboard-navbar-breadcrumb.component.ts
- blog-post-editor-pre-logo-action.component.ts

Conclusion:

Need to add unit tests.

Production code itself appears correct.

---

# Local Coverage Commands

Discovered Oppia supports:

```bash
python -m scripts.run_frontend_tests --check_coverage
```

Also useful:

```bash
python -m scripts.run_frontend_tests \
--run_on_changed_files_in_branch \
--check_coverage
```

This allows reproducing GitHub coverage failures locally.

---

# Current Status

Current PR:

#26275

Outstanding work:

- Add missing frontend unit tests.
- Verify local coverage.
- Push changes.
- Resolve merge conflicts with latest develop.
- Wait for reviewer approval.

---

# Lessons Learned

## Git

- Avoid force-pushing rewritten review history.
- Cherry-pick only clean commits when restarting.
- Verify commits before pushing.

## Oppia Workflow

- Share strict-check proof before requesting assignment.
- Use @username PTAL for reviewer requests.
- Keep project-wide files unchanged.
- Run local coverage checks before pushing.
- Resolve review comments with small focused commits.

## Debugging

Always distinguish between:

- Compilation failures
- Unit test failures
- Coverage failures

Passing tests do not guarantee passing coverage checks.

---

# Current Branch

strict-checks-blog-dashboard-v3

---

# PR

#26275

---

# Next Steps

1. Finish missing unit tests.
2. Run:

```bash
python -m scripts.run_frontend_tests \
--run_on_changed_files_in_branch \
--check_coverage
```

3. Resolve merge conflicts.
4. Push updates.
5. Request reviewer again using:

@Vir-8 PTAL

(or assigned reviewer)

6. Wait for final review.