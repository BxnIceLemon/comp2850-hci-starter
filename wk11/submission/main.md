# Week 9-10 Assessment Report: Evaluation & Redesign

Module: COMP2850

Date: 2025-12-29

Status: Final Submission

------

## Section 1: Evaluation Plan

### 1.1 Test Tasks & Scenarios

Based on the project's inclusion goals, four tasks were designed to stress-test the application's "Dual-Path"
architecture (HTMX vs. No-JS) and keyboard accessibility.

1. **Task 1 (Filter)**: Find tasks containing "Party".
    - *Focus*: Search efficiency and affordance in No-JS mode.
2. **Task 2 (Add Task - Validation)**: Attempt to add a task with a blank title.
    - *Focus*: Error recovery and focus management (WCAG 3.3.1).
3. **Task 3 (Edit Task)**: Fix a typo in an existing task.
    - *Focus*: Interaction flow and functionality parity for No-JS users.
4. **Task 4 (Delete Task)**: Delete the task "Old newspapers".
    - *Focus*: Error prevention (WCAG 3.3.4) and psychological safety.

### 1.2 Metrics

We collected both quantitative and qualitative data:

- **Time-on-Task (Server)**: Measured via server-side instrumentation to isolate application performance from network
  latency.
- **Success Rate**: Binary (Pass/Fail) completion of the goal.
- **Confidence Score (1-5)**: Self-reported rating to measure user trust and psychological safety.
- **Error Rate**: Frequency of unforced errors or functional blocks.

### 1.3 Protocol

Pilots were conducted with peer participants using a counter-balanced approach (HTMX first vs. No-JS first). Verbal
consent was obtained, and data was anonymized using Session IDs (e.g., `P1_e06zfc`) to comply with ethical standards.

------

## Section 2: Findings & Analysis (Week 9)

### 2.1 Quantitative Results

Data from 4 participants (3 HTMX, 1 No-JS) revealed significant disparities in safety and functionality.

| **Task**        | **HTMX (n=3)**   | **No-JS (n=1)** | **Insight**                                                   |
|-----------------|------------------|-----------------|---------------------------------------------------------------|
| **T1 (Filter)** | ~3.7ms           | ~2.0ms          | Server is fast; UX difference is client-side.                 |
| **T2 (Add)**    | 100% Success     | 100% Success    | Both groups recovered successfully.                           |
| **T3 (Edit)**   | **100% Success** | **0% Success**  | **Critical Blocker**: No-JS user trapped in edit mode.        |
| **T4 (Delete)** | ~2.3ms           | **1.0ms**       | No-JS deletion was "instant" because it skipped confirmation. |

**Key Insight**: While Task 4 (Delete) appears faster in No-JS, this speed indicates a **safety violation** (skipping
confirmation). Task 3 (Edit) was a complete functional failure for the No-JS user.

### 2.2 Qualitative Themes

Observations confirmed the quantitative data:

1. **Safety Gap**: The No-JS participant expressed anxiety during deletion: *"Wait, did I delete it? It didn't ask
   me..."*
2. **Functional Trap**: The No-JS participant could not cancel the edit: *"I clicked cancel but it didn't do anything...
   is it broken?"*

### 2.3 Evidence Chains (Before Fix)

Chain 1: The Safety Gap (Delete)

In Week 9, clicking "Delete" in No-JS mode triggered an immediate POST request. The list item vanished instantly without
warning, violating WCAG 3.3.4 (Error Prevention).

![nojs-delete](C:\Users\ASUS\OneDrive\School Work\University\Year 2 Semester 1\In progress\285001 软件工程\Portfolio
Assignment 2 - HCI\comp2850-hci-starter\wk11\submission\assets\nojs-delete.png)

*Figure 1: Task list showing item removed immediately after click (No-JS), causing user anxiety.*

Chain 2: The Functional Trap (Edit)

The "Cancel" button was implemented as a generic \<button\> relying on hx-get. Without JavaScript, this button was
inert, trapping the user in the edit form.

![nojs-broken-cancel](C:\Users\ASUS\OneDrive\School Work\University\Year 2 Semester 1\In progress\285001 软件工程\Portfolio
Assignment 2 - HCI\comp2850-hci-starter\wk11\submission\assets\nojs-broken-cancel.png)

*Figure 2: The "Cancel" button is visually interactive but functionally inert in No-JS mode.*

### 2.4 Positive Accessibility Finding

Despite the issues above, the validation strategy proved robust. The No-JS Error Summary successfully managed focus,
meeting **WCAG 3.3.1**.

![nojs-validation-success](C:\Users\ASUS\OneDrive\School Work\University\Year 2 Semester 1\In progress\285001
软件工程\Portfolio Assignment 2 - HCI\comp2850-hci-starter\wk11\submission\assets\nojs-validation-success.png)

*Figure 3: Accessible Error Summary guiding No-JS users to the invalid field.*

------

## Section 3: Redesign & Implementation (Week 10)

### 3.1 Prioritisation

Based on the Inclusion-Severity Matrix, two issues were selected for immediate remediation:

1. **Priority 1 (Critical Safety)**: No-JS Delete Confirmation (Backlog ID 14).
2. **Priority 2 (Functional Blocker)**: Broken Edit Cancel Button (Backlog ID 15).

### 3.2 Fix 1: No-JS Delete Confirmation

**Solution**: We implemented an "Interceptor Route" pattern. For No-JS users, the "Delete" button now links to a
dedicated confirmation page (`GET /tasks/{id}/delete/confirm`) instead of submitting a POST request immediately.

Implementation Evidence:

![fix-delete-confirm-nojs](C:\Users\ASUS\OneDrive\School Work\University\Year 2 Semester 1\In progress\285001
软件工程\Portfolio Assignment 2 - HCI\comp2850-hci-starter\wk11\submission\assets\fix-delete-confirm-nojs.png)

Figure 4: Safety gap closed via a dedicated confirmation route for No-JS users.

### 3.3 Fix 2: Robust Cancel Button

**Solution**: To fix the broken button while maintaining visual consistency with the "Save" button, we utilized the
HTML5 `formaction` attribute.

**Code Change**:

```html

<button hx-get="...">Cancel</button>

<button type="submit"
        formaction="/tasks"
        formmethod="get"
        hx-get="/tasks/{{ task.id }}/view"
        class="outline">
    Cancel
</button>
```

Implementation Evidence:

![fix-cancel-code](C:\Users\ASUS\OneDrive\School Work\University\Year 2 Semester 1\In progress\285001 软件工程\Portfolio
Assignment 2 - HCI\comp2850-hci-starter\wk11\submission\assets\fix-cancel-code.png)

Figure 5: DevTools showing the formaction attribute, enabling No-JS navigation while keeping \<button\> styling.

------

## Section 4: Verification & Reflection

### 4.1 Re-Verification

Manual regression testing confirmed that the fixes are effective:

- [x] **No-JS**: Clicking "Delete" now leads to the confirmation page.
- [x] **No-JS**: Clicking "Cancel" now correctly navigates back to the list view.
- [x] **Automated Audit**: Axe tools confirmed no new accessibility violations were introduced.

![wk10-axetool](C:\Users\ASUS\OneDrive\School Work\University\Year 2 Semester 1\In progress\285001 软件工程\Portfolio
Assignment 2 - HCI\comp2850-hci-starter\wk11\submission\assets\wk10-axetool.png)

*Figure 6: Automated accessibility audit confirming compliance.*

### 4.2 Reflection

**Process**: The transition from evaluation to redesign highlighted the importance of "Progressive Enhancement".

**Future Work**: While safety and functionality are fixed, usability improvements like "Filter Persistence" (keeping
search terms after reload) remain in the backlog for Semester 2.