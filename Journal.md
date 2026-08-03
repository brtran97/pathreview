## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/151

**Issue title:** Bias detector patterns are too narrow to match common phrasings
 #151

 **Issue Selection:**
 I am able to understand what the issue is from the comments in the github issue and can understand what the corrected version is supposed to do (pass the unit tests for bias detection). I was able to find where the issue exists from the tags that say it is in the safety folder and it is inside the bias detector.
 This is my first open source contribution, so I am choosing a tier 1 issue in order to get more experience. I have read the actual code in the file where the issue exists. I have found the unit tests that call on the bias detector so I can understand the context behind how it works and how it is being tested. There are no other students in my session working on this problem but there are a good amount of students in other sections working on it going by the comments in the github issue. This issue should take me somewhere between 4-8 hours to implement by my estimate so I am confident I will be able to test and submit the PR by week 9. I have also verified that this issue has no open blockers or dependencies on other unresolved issues. These meet all the requirements for "Is This Issue Right for Me?"

**Tier:** [x] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**
In the safety section folder of the project the file **bias_detector.py** uses regex patterns in order to detect phrasing that expresses a certain type of meaning however. The regex patterns are too strict and narrow so it fails to detect bias in 9 units tests. If the regex can be adjusted to pass all the unit tests then the bug will be successfully fixed.

**Branch name:** https://github.com/brtran97/pathreview/tree/fix/151-bias-detector-narrow-patterns

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [x] Issue added to cohort ledger

## Week 8 — Reproduction & solution planning

**Reproduction commit link:** https://github.com/brtran97/pathreview/commit/87268077bbfacb5fff7ff842d056fe1203c3c753

**Reproduction summary:**
I reproduced the issue by running the bias detector unit tests in my local
environment with `.venv/bin/python -m pytest tests/unit/test_bias_detector.py -v`
(equivalent to `make test-unit` scoped to this file) or with vscodes pytest GUI. I observed **9 failed, 23
passed**, exactly matching the counts described in issue #151. The failures confirm
that `BiasDetector.detect_bias(...)` returns `(False, '')` for natural phrasings that
should be flagged (e.g. dismissive bootcamp language and age-based assumptions),
because the regex patterns in `safety/bias_detector.py` are too narrow.

The 9 failing tests:

```
FAILED tests/unit/test_bias_detector.py::TestBiasDetector::test_dismissive_bootcamp_language_detected
FAILED tests/unit/test_bias_detector.py::TestBiasDetector::test_bootcamp_lacks_rigor_detected
FAILED tests/unit/test_bias_detector.py::TestBiasDetector::test_demographic_assumption_age_detected
FAILED tests/unit/test_bias_detector.py::TestBiasDetector::test_coding_bootcamp_variant
FAILED tests/unit/test_bias_detector.py::TestBiasDetector::test_developer_vs_programmer_distinction
FAILED tests/unit/test_bias_detector.py::TestBiasDetector::test_multiple_bias_indicators
FAILED tests/unit/test_bias_detector.py::TestBiasDetector::test_negative_educational_claim
FAILED tests/unit/test_bias_detector.py::TestBiasDetector::test_rich_poor_assumption
FAILED tests/unit/test_bias_detector.py::TestBiasDetector::test_assumption_vs_observation
========================= 9 failed, 23 passed in 0.18s =========================
```

**PLAN.md link:** https://github.com/brtran97/pathreview/commit/2b00be758a03d6e6a935ee9016aeaa33734eeb6f

**Blockers or open questions:**
None — the failing tests clearly specify the intended behavior, so the path forward is well defined.

## Week 9 — Solution building & PR submission

### Check-in 1 (mid-week)

**Current progress:**
Implemented the fix from PLAN.md in two iterative commits on
`safety/bias_detector.py`. (1) Broadened `DISMISSIVE_PATTERNS` into an
education-keyword + nearby-negative-predicate structure so natural phrasings
("bootcamp graduates can't write production code", "bootcamp education lacks
fundamentals") match without requiring an exact word sequence — this fixed 7 of the
9 failing tests. (2) Broadened `DEMOGRAPHIC_PATTERNS` to accept plural subjects and
subject anchors beyond "person from" (e.g. "developers from poor backgrounds"),
fixing the remaining 2. All 32 bias-detector tests now pass.

**Next steps:**
Run the full self-review (`make check` / `make test-unit`), open a draft PR into
upstream `ascherj/pathreview`, request peer feedback in Slack, then mark ready for
review and submit.

**Blockers:**
None.

---

### Check-in 2 (end of week)

**PR link:** https://github.com/ascherj/pathreview/pull/615

**Branch:** `fix/151-bias-detector-narrow-patterns`

**What you built:**
Broadened the regex patterns in `safety/bias_detector.py` so `BiasDetector.detect_bias()`
recognizes common natural phrasings of educational-background dismissiveness and
demographic assumptions — not just near-exact phrase sequences. The `detect_bias()`
signature and return shape (`tuple[bool, str]`) are unchanged; only detection coverage
improved, and positive/neutral mentions of the same topics stay unflagged.

**Tests added or updated:**
No new tests written — the fix targets the 9 pre-existing failing tests in
`tests/unit/test_bias_detector.py`, which now pass (suite is 32/32). Those tests cover:
dismissive educational-background phrasings (bootcamp/self-taught/online-course +
"can't"/"lacks"/"inadequate"/"not equal"), demographic assumptions (age, socioeconomic
background, immigrant/international/foreign developers), and precision guardrails that
positive/neutral mentions (e.g. "your bootcamp background shows strong fundamentals")
remain unflagged.

**Self-review confirmation:** [x] make check passes  [x] make test-unit passes
(Note: the repo has pre-existing, unrelated failures in other modules. Verified my
change introduces none: full unit suite went from 53 failed → 44 failed (my 9 fixes),
and `safety/bias_detector.py` passes ruff, black, and mypy cleanly.)

**Draft PR feedback received from:** None