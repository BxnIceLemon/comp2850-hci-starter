# Pilot Findings Analysis — Week 9

Participants: 4 (3 HTMX, 1 No-JS)

Date range: 2025-11-22~

------

## Quantitative Summary

### Task 1: Filter and Complete

| Metric                | HTMX (n=3) | No-JS (n=1) | Overall |
|-----------------------|------------|-------------|---------|
| Mean Server Time (ms) | 3.7        | 2.0         | 3.4     |
| Success rate          | 100%       | 100%        | 100%    |
| Mean errors           | 0.0        | 0.0         | 0.0     |
| Mean confidence       | 5.0/5      | 4.0/5       | 4.8/5   |

Interpretation: Task 1 was successful for all participants. Server response times are negligible (<5ms), confirming
highly performant filtering. HTMX users reported higher confidence due to the instant visual feedback (list updating
while typing), while the No-JS participant had to manually submit the form.

------

### Task 2: Add New Task (Validation Stress Test)

| Metric                | HTMX (n=3) | No-JS (n=1) | Overall |
|-----------------------|------------|-------------|---------|
| Mean Server Time (ms) | 14.7       | 8.0         | 13.0    |
| Success rate          | 100%       | 100%        | 100%    |
| Mean errors           | 1.0        | 1.0         | 1.0     |
| Mean confidence       | 5.0/5      | 4.0/5       | 4.8/5   |

Interpretation: All participants successfully triggered the validation error and recovered. The error rate of 1.0 is
expected (part of the task script). HTMX users benefited from inline error messages (retaining focus), while the No-JS
participant relied on the error banner after a page reload.

------

### Task 3: Delete Task

| Metric                | HTMX (n=3) | No-JS (n=1) | Overall |
|-----------------------|------------|-------------|---------|
| Mean Server Time (ms) | 2.3        | 1.0         | 2.0     |
| Success rate          | 100%       | 100%        | 100%    |
| Mean errors           | 0.0        | 0.0         | 0.0     |
| Mean confidence       | 5.0/5      | 2.0/5       | 4.3/5   |

Interpretation: The most significant disparity in user experience. While success rates are 100%, the No-JS
participant reported significantly lower confidence (2/5) due to the lack of a confirmation dialog ("Wait, did I delete
it?").

------

### Task 4: Edit Task

| Metric                | HTMX (n=3) | No-JS (n=1) | Overall |
|-----------------------|------------|-------------|---------|
| Mean Server Time (ms) | 2.3        | 1.0         | 2.0     |
| Success rate          | 100%       | 100%        | 100%    |
| Mean errors           | 0.0        | 0.0         | 0.0     |
| Mean confidence       | 5.0/5      | 4.0/5       | 4.8/5   |

Interpretation: Inline editing (HTMX) felt "smooth" to users. The No-JS experience was functional but involved full
page reloads. A critical issue was identified with the "Cancel" button in No-JS mode (see Qualitative Themes).

------

## Qualitative Themes

### Theme 1: Confirmation Gap in No-JS

Evidence: No-JS participant expressed surprise and anxiety during Task 3 (Delete).

Quotes:

- P2 (No-JS): "Wait, did I delete it? It didn't ask me 'are you sure?'"
- P1 (HTMX): "Clicked 'OK' without hesitation."

Design implication: The "Dual-Path" architecture works functionally, but the lack of deletion confirmation in the
No-JS path is a major UX risk. We should implement a dedicated "Confirm Delete" page for non-JS users.

### Theme 2: Cancel Button Malfunction (No-JS)

Evidence: During Task 4, the No-JS participant attempted to cancel an edit but the button was unresponsive.

Quotes:

- P2 (No-JS): "I clicked cancel but it didn't do anything."

Design implication: The "Cancel" button is currently a simple button relying on HTMX. It needs to be converted to a
link (`<a href="/tasks">`) to ensure it functions as a "Back" button when JavaScript is disabled.

### Theme 3: Error Visibility & Accessibility

Evidence: Both modes handled errors well, but in different ways.

Quotes:

- P2 (No-JS): "Oh, there's a big red box at the top... Clicked the link, focus jumped to input."
- P3 (Keyboard): "Status message 'Title is required' was injected... focus remained on input."

Design implication: The current error handling strategy (Inline for HTMX, Summary for No-JS) is effective and
accessible.

------

## Accessibility Observations

### Screen Reader / Keyboard (P3)

- ✅ Navigation: "Skip to main content" link and general tab order are working well.
- ✅ Feedback: Status messages are correctly injected for screen readers in HTMX mode.

### No-JS Fallbacks (P2)

- ✅ Error Recovery: The Error Summary at the top of the page successfully guided the user to the input field.
- ❌ Interactive Elements: Elements that look like buttons (e.g., Cancel) but rely solely on JS event listeners are
  broken in this mode.

------

## Prioritised Issues

Based on the pilot data, we have identified the following priorities for Week 10 redesign:

1. High (Critical): Fix "Cancel" button in Edit form. Currently broken for No-JS users. (Evidence: P2 Task 4)
2. High (UX/Safety): Add Delete Confirmation for No-JS. Users feel unsafe without it. (Evidence: P2 Task 3
   Confidence score)