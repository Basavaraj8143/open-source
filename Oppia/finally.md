# 🧑‍💻 Open Source Contribution — Oppia

## 📌 PR Details

- **Repository:** https://github.com/oppia/oppia
- **Issue:** #26108
- **Pull Request:** #26866
- **Title:** Fix part of #26108: Fix strict template checks for blog dashboard files
- **Status:** ✅ Merged

---

# 🔄 Post Merge-Conflict Timeline

## 1. Merge Conflict Resolved

After my PR received approvals, the `develop` branch advanced, causing merge conflicts.

Instead of rebasing or force-pushing, I followed Oppia's contribution guidelines and:

- Pulled the latest `develop`
- Merged `develop` into my feature branch
- Resolved the merge conflicts locally
- Committed the merge
- Pushed the updated branch

This preserved the review history and approvals.

---

## 2. CI Started Again

After pushing the merge commit:

- Around **180 GitHub Actions checks** started.
- Most of them passed successfully.

Eventually only **three checks failed**.

```
Acceptance (lesson-creator/create-a-collection)

Acceptance (topic-manager/add-questions-from-topic-editor)

Check that all necessary tests pass
```

---

## 3. Reviewer Response

The reviewer replied:

> You need to report these flakes then we can re-run the tests.

and shared Oppia's CI debugging guide.

This indicated that maintainers expected contributors to investigate whether failures were caused by the PR or by existing flaky tests.

---

## 4. Investigating CI Failures

Initially, I saw logs like:

```
ERROR: Elasticsearch exited unexpectedly

Portserver failed to shut down

Firebase Emulator shutting down

Redis stopped
```

At first these appeared to be the cause.

After carefully reading the logs, I learned these messages were generated **during cleanup after the test had already failed**, not the root cause.

---

## 5. Finding the Actual Failure

After inspecting the acceptance test logs, I found the real error:

```text
TimeoutError:
Element

<button ... e2e-test-save-question-button>

"SAVE"

took too long to be clickable.

Detected reasons:

Element is disabled.
```

Stack trace:

```
BaseUser.waitForElementToBeClickable()

↓

BaseUser.clickOnElement()

↓

BaseUser.saveQuestion()

↓

Topic Manager Acceptance Test
```

The failure occurred because the **SAVE** button never became enabled.

---

## 6. Investigating Existing Flakes

Following Oppia's guidelines, I searched GitHub Issues.

### Existing Issue #26605

```
Acceptance (topic-manager/add-questions-from-topic-editor)

Timeout waiting for selector
```

Although it involved the same acceptance suite, the failure signature was different.

---

### Existing Issue #25929

```
"SAVE" took too long to be clickable
```

This issue matched my failure much more closely.

Both failures involved:

- SAVE button remaining disabled
- Puppeteer timing out
- Question editing workflow

This strongly suggested that the acceptance suite already had flaky behavior.

---

## 7. Understanding the CI Failure

Important realization:

The acceptance test itself failed.

Everything after that:

- Elasticsearch shutdown
- Firebase shutdown
- Redis shutdown
- Portserver timeout

was simply CI cleaning up after the failed test.

Those logs were **not** the root cause.

---

## 8. Outcome

The maintainers completed the review process.

The PR was successfully merged.

Commit:

```
d9d66af
```

Author:

```
Basavaraj8143
```

Commit message:

```
Fix part of #26108:
Fix strict template checks for blog dashboard files.
```

---

# 📚 What I Learned

### Technical

- How Oppia handles merge conflicts.
- Why force-pushing after reviews should be avoided.
- Reading GitHub Actions logs effectively.
- Distinguishing cleanup errors from the actual test failure.
- Understanding Puppeteer acceptance test failures.
- Investigating flaky CI failures before requesting reruns.

---

### Open Source Workflow

- Preserving review history.
- Following maintainer instructions.
- Using GitHub Issues to identify existing flakes.
- Comparing stack traces before creating a new issue.
- Understanding CI infrastructure in large repositories.

---

# 🎉 Result

✅ Oppia Pull Request #26866 merged successfully.

This contribution strengthened my understanding of:

- Large-scale open source collaboration
- CI debugging
- Acceptance testing
- GitHub review workflow
- Working with maintainers
