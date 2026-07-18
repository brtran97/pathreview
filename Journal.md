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