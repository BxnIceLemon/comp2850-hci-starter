# Quantitative Analysis — Week 10

## Task Success Rates

| **Task**              | **HTMX Success (n=3)** | **No-JS Success (n=1)** | **Overall** |
|-----------------------|------------------------|-------------------------|-------------|
| T1: Filter & Complete | 100% (3/3)             | 100% (1/1)              | 100%        |
| T2: Add Task          | 100% (3/3)             | 100% (1/1)              | 100%        |
| **T3: Edit Inline**   | 100% (3/3)             | **0% (0/1)**            | 75%         |
| T4: Delete            | 100% (3/3)             | 100% (1/1)              | 100%        |

**Interpretation**:

- **T3 (Edit)** failed in No-JS mode. The participant was unable to cancel the edit action (button non-functional),
  requiring browser navigation to escape. This is a critical functional blocker.
- **T4 (Delete)** shows 100% completion, but this masks a safety issue (see Confidence Ratings below).

## Mean Time-on-Task (Observed)

| **Task**       | **HTMX (Mean)** | **No-JS (Observed)** | **Insight**                                                      |
|----------------|-----------------|----------------------|------------------------------------------------------------------|
| T1: Filter     | ~9s             | 15s                  | No-JS slower due to full page reload logic.                      |
| T2: Add Task   | ~18s            | 30s                  | No-JS slower but functional.                                     |
| **T3: Edit**   | ~15s            | **45s+**             | **Blocked**: No-JS user trapped by broken cancel button.         |
| **T4: Delete** | ~6s             | **2s**               | **Too Fast**: No-JS deletion was instant (skipped confirmation). |

**Interpretation**: The "efficiency" of T4 in No-JS mode (2s vs 6s) is actually a heuristic violation (Safety), as it
bypasses the confirmation step present in the HTMX path.

## Error Rates

| **Task**     | **HTMX Error Rate** | **No-JS Error Rate** | **Issue Type**                                 |
|--------------|---------------------|----------------------|------------------------------------------------|
| T1: Filter   | 0%                  | 0%                   | -                                              |
| T2: Add Task | 0%                  | 0%                   | (Validation errors were forced by protocol)    |
| **T3: Edit** | 33%                 | **100%**             | **Functional**: Cancel button broken in No-JS. |
| T4: Delete   | 0%                  | 0%                   | -                                              |

**Interpretation**: The 100% error rate in T3 (No-JS) confirms the high priority of the "Broken Cancel Button" backlog
item.

## Confidence Ratings (1-5 Scale)

| **Task**       | **HTMX (Mean)** | **No-JS (Observed)** | **Gap**  |
|----------------|-----------------|----------------------|----------|
| T1: Filter     | 5.0             | 4.0                  | -1.0     |
| T2: Add Task   | 5.0             | 4.0                  | -1.0     |
| T3: Edit       | 5.0             | 3.0                  | -2.0     |
| **T4: Delete** | **5.0**         | **2.0**              | **-3.0** |

**Interpretation**:

- **T4 (Delete)**: The significant drop in confidence (2/5) for the No-JS participant confirms the "Safety Gap". The
  user successfully deleted the item but felt anxious about the lack of feedback/confirmation.
- **T3 (Edit)**: Lower confidence reflects confusion over the non-responsive UI elements.