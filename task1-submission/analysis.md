# Quantitative Analysis — Week 9 Pilot Data

Dataset: `metrics.csv` (Server-side logs)
Participants: N=4 (3 HTMX, 1 No-JS)

---

## 1. System Performance (Server Time)

*Analysis of server processing time (`ms`). High MAD would indicate unstable server performance.*

| Task       | Mode  | Median (ms) | MAD (ms) | Completion Rate | Error Rate |
|:-----------|:------|:------------|:---------|:----------------|:-----------|
| T1: Filter | HTMX  | 1.5         | 0.5      | 100%            | 0%         |
|            | No-JS | 2.0         | 0.0      | 100%            | 0%         |
| T2: Add    | HTMX  | 15.0        | 1.0      | 100%            | 100%*      |
|            | No-JS | 8.0         | 0.0      | 100%            | 100%*      |
| T3: Edit   | HTMX  | 2.0         | 0.0      | 100%            | 0%         |
|            | No-JS | 1.0         | 0.0      | 100%            | 0%         |
| T4: Delete | HTMX  | 3.0         | 0.0      | 100%            | 0%         |
|            | No-JS | 1.0         | 0.0      | 100%            | 0%         |

Notes:

* *\*Error Rate for T2 is 100% by design (Validation Stress Test).*
* *\*\*T3 No-JS shows 0% system errors, but failed functionally (user unable to cancel).*
* *MAD (Median Absolute Deviation) < 1.0 indicates highly consistent server performance.*

---

## 2. User Performance (Observed)

*Time measured from task start to completion (Human time estimates).*

| Task       | HTMX (Median) | No-JS (Observed) | Insight                                |
|:-----------|:--------------|:-----------------|:---------------------------------------|
| T1: Filter | ~9s           | 15s              | No-JS slower due to reload cycle.      |
| T2: Add    | ~18s          | 30s              | HTMX validation cycle is faster.       |
| T3: Edit   | ~15s          | Failed           | No-JS user trapped by broken UI.       |
| T4: Delete | ~6s           | 2s               | No-JS "too fast" (skips safety check). |

---

## 3. Confidence Scores (Likert 1-5)

| Task       | HTMX | No-JS | Gap  |
|:-----------|:-----|:------|:-----|
| T1: Filter | 5.0  | 4.0   | -1.0 |
| T2: Add    | 5.0  | 4.0   | -1.0 |
| T3: Edit   | 5.0  | 3.0   | -2.0 |
| T4: Delete | 5.0  | 2.0   | -3.0 |

Conclusion:
While System Performance (Median/MAD) is stable across both modes, User Confidence drops significantly in No-JS mode for
safety-critical tasks.