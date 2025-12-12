# Evaluation Protocol — Week 9 Pilots

**System**: COMP2850 Task Manager
**Focus**: Usability & Accessibility Parity (HTMX vs No-JS)
**Date**: 2025-11-22

---

## 1. Setup & Consent

**Before the session**:

1. **Assign Group**: Odd participant ID = HTMX (JS On); Even participant ID = No-JS (JS Off).
2. **Prepare Data**: Ensure at least 15 tasks exist in the database.
3. **Privacy Check**: Ensure `metrics.csv` is clean or backed up.

**Consent Script**:
> "Hi, I'm testing the robustness of this Task Manager app. I'm not testing *you*, I'm testing the *code*.
>
> I will ask you to perform 4 tasks.
> I am recording **anonymous** usage data (like how long a click takes) to a local file.
> No personal info is stored. You can stop at any time.
>
> Is that okay with you?"

---

## 2. Execution Flow

**Step 1: The Filter Test (Task 1)**

- *Prompt*: "Find all tasks about 'Party'. How many are there?"
- *Observer Check*: Did they use the filter input or scroll manually? Did they notice the count?

**Step 2: The Error Test (Task 2)**

- *Prompt*: "Try to add a task without typing a title first. See what happens. Then add 'Call Mom'."
- *Observer Check*: Did they notice the error message immediately? (HTMX: Inline / No-JS: Summary)

**Step 3: The Edit Test (Task 3)**

- *Prompt*: "Fix the spelling of 'By milk' to 'Buy milk'."
- *Observer Check*: Did the inline form confuse them? Did they try to Cancel?

**Step 4: The Delete Test (Task 4)**

- *Prompt*: "Delete the 'Old newspapers' task."
- *Observer Check*:
    - (If HTMX): Did the confirm dialog appear?
    - (If No-JS): Did they hesitate or express surprise at the immediate deletion?

---

## 3. Debrief Questions

After all tasks are done, ask:

1. "On a scale of 1-5, how confident did you feel deleting that task?"
2. "Did anything behave differently than you expected?"
3. "Did you feel the interface was fast enough?"

---

## 4. Observer Logging

*Record the following in `pilot-notes/P{id}_notes.md`:*

- Success/Failure for each task.
- Any verbal quotes (e.g., "Wait, what happened?").
- Any critical errors (e.g., stuck on a page).