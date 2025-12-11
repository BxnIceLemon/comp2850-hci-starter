# Evaluation Protocol — Week 9 Pilots

**System**: COMP2850 Task Manager
**Focus**: Usability & Accessibility Parity (HTMX vs No-JS)

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
- *Observer Check*: Did they use the filter input or scroll manually?

**Step 2: The Error Test (Task 2)**

- *Prompt*: "Try to add a task without typing a title first. See what happens. Then add 'Call Mom'."
- *Observer Check*: Did they notice the red error text immediately?

**Step 3: The Edit Test (Task 4)**

- *Prompt*: "Fix the spelling of 'By milk' to 'Buy milk'."
- *Observer Check*: Did the inline form confuse them?

**Step 4: The Delete Test (Task 3)**

- *Prompt*: "Delete the 'Old newspapers' task."
- *Observer Check*: (If HTMX) Did the confirm dialog surprise them? (If No-JS) Did they hesitate?

---

## 3. Debrief & Debias

**Post-Test Questions**:

1. "Which task felt the slowest?"
2. "Did you notice the status messages at the top/bottom?"
3. (If No-JS) "Did the page reloading bother you?"

**Closing**:
"Thank you. If you want me to delete your session data, just let me know your Session ID is [provide ID]."