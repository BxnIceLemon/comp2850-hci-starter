# Pilot Findings Analysis — Week 9

**Participants**: 4 (3 HTMX, 1 No-JS)
**Date range**: 2025-11-22
**Dataset**: Server logs (`metrics.csv`) + Observation Notes (`pilot-notes/`)

---

## 1. Quantitative Summary

*(See `analysis.md` for full statistical breakdown including Median and MAD)*

| Task           | Completion Rate | Error Rate      | Confidence Gap (No-JS) |
|:---------------|:----------------|:----------------|:-----------------------|
| **T1: Filter** | 100%            | 0%              | -1.0                   |
| **T2: Add**    | 100%            | 100%*           | -1.0                   |
| **T3: Edit**   | 100%            | **0%** (System) | **-2.0**               |
| **T4: Delete** | 100%            | 0%              | **-3.0** (Critical)    |

*\*High error rate on T2 due to validation stress test protocol.*

---

## 2. Evidence-Based Findings

### Finding 1: Safety Violation in No-JS Deletion (Priority 1)

**Evidence**:

- **Quantitative**: P2 (No-JS) on Task 4 (Delete) had a server time of **1.0 ms** (Median) vs HTMX Median **3.0 ms**.
  This "too fast" time confirms the confirmation dialog step was skipped.
- **Confidence**: P2 reported **2/5** confidence (Gap: -3.0 vs HTMX), the lowest score in the study.
- **Qualitative**: Pilot notes (P2, Task 4): *"Wait, did I delete it? It didn't ask me 'are you sure?'"*

**Root Cause**:
The Delete button relies on `hx-confirm` (HTMX attribute) for safety. In No-JS mode, this attribute is ignored, and the
form submits a `POST` request immediately.

**Impact**:
**WCAG 3.3.4 (Error Prevention)** failure. Reversible actions or confirmation are mandatory for data deletion.

**Backlog Item**:
ID #14 "Add Delete Confirmation Page for No-JS" (Priority: Critical, Effort: 1.5h).

---

### Finding 2: Functional Block in Edit Form (Priority 1)

**Evidence**:

- **Quantitative**: Task 3 (Edit) shows **100% Functional Failure** for No-JS. While the *edit* itself was saved (
  Success), the user failed the "Cancel" sub-task.
- **Qualitative**: Pilot notes (P2, Task 4): *"I clicked cancel but it didn't do anything."*

**Root Cause**:
The "Cancel" button is implemented as `<button hx-get="...">`. Without JavaScript, this element is semantically a button
that does nothing (no `type="submit"` and no `formaction`), rendering it inert.

**Impact**:
**WCAG 2.1.1 (Keyboard)** failure. All functionality must be operable through a keyboard interface (and by extension,
without client-side scripting).

**Backlog Item**:
ID #15 "Fix Edit Cancel Button for No-JS" (Priority: High, Effort: 0.5h).

---

### Finding 3: Validation Strategy is Effective (Positive)

**Evidence**:

- **Quantitative**: 100% recovery rate on Task 2 (Add Task) for both groups.
- **Qualitative**: P2 (No-JS) noted: *"Oh, there's a big red box at the top... Clicked the link, focus jumped to
  input."* P3 (Keyboard/HTMX) confirmed inline error announcement.

**Root Cause**:
The "Dual-Path" validation (Inline for HTMX, Error Summary for No-JS) correctly implements **WCAG 3.3.1 (Error
Identification)** and manages focus.

**Impact**:
Validates the current architectural approach; no redesign required for validation features.

---

## 3. Accessibility Observations

### Screen Reader (P3 - Simulated)

- **Observation**: Status messages (e.g., "Task updated") injected into the DOM via HTMX were announced correctly.
- **Gap**: The "Cancel" button in No-JS mode would be confusing as it is focusable but performs no action.

### Keyboard Navigation (P3)

- **Observation**: "Skip to main content" link works correctly. Tab order is logical.
- **Gap**: Focus management after deleting a task (HTMX mode) is currently missing (focus is lost/reset to body), which
  is a minor usability friction.

---

## 4. Prioritised Issues for Week 10

1. **[Critical] No-JS Delete Safety**: Implement interceptor pattern (Confirmation Page).
2. **[High] No-JS Edit Functionality**: Fix Cancel button using `formaction` attribute.