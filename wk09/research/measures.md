# Evaluation Metrics — Week 9 (Customized)

## Objective Metrics (Hard Data)

### 1. Recovery Time (Error Handling)

**Definition**: Time elapsed between triggering a validation error (Task 2) and successfully submitting the corrected
form.
**Why**: Measures how helpful our error messages are. Long recovery time = confusing error UI.
**Source**: Timestamp difference in logs between `validation_error` and subsequent `success`.

### 2. Mode Latency Comparison

**Definition**: Average time-on-task for HTMX participants vs. No-JS participants.
**Hypothesis**: HTMX should be faster for Task 1 (Filter) and Task 4 (Edit) because it avoids full page reloads.
**Source**: `duration_ms` column in `metrics.csv`.

### 3. Navigation Efficiency (Clicks/Keystrokes)

**Definition**: Number of interactions required to complete Task 3 (Delete).
**Expected**:

- HTMX: 2 clicks (Delete button + Confirm OK).
- No-JS: 1 click (Delete button).
  **Why**: Validates the trade-off we documented in Lab 2 (Security vs. Convenience).

---

## Subjective Metrics (User Feeling)

### 4. Error Clarity Rating

**Question**: "When you made a mistake adding the task, how clear was the error message?"
**Scale**: 1 (Very Confusing) to 5 (Very Clear).
**Why**: Validates our WCAG 3.3.1 implementation (Color + Text + Location).

### 5. Confidence Score

**Question**: "How confident are you that the task was actually deleted?"
**Scale**: 1 (Not confident) to 5 (Very confident).
**Why**: Tests if our status messages ("Task deleted") are noticeable enough.

---

## Observational Codes (For Note Taking)

- **Hesitation**: Pausing > 3 seconds.
- **Mode Confusion**: Trying to click "Cancel" but hitting "Back" button.
- **Blindness**: Missing the status notification bar (banner blindness).
- **A11y-Fail**: Screen reader talking over the status message.