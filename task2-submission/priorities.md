# Redesign Priorities & Scoring Matrix — Week 10

**Rationale**: Priorities are determined using an Inclusion-Severity Matrix. We prioritise issues that score highest on
**Functional Severity** (Blockers/Safety) and **Inclusion Risk** (Who is excluded?).

## 1. The Scoring Matrix

**Scoring Legend**:

* **Severity (1-3)**: 1 = Cosmetic/Minor; 2 = Major Friction; 3 = Critical Blocker or Safety Hazard.
* **Inclusion Risk (1-3)**: 1 = Situational; 2 = Specific Assistive Tech; 3 = Systemic (e.g., No-JS, Keyboard, No
  Screen).
* **Priority Score**: Severity + Inclusion Risk (Max 6).

| Backlog ID | Issue Description       | Severity (1-3)  | Inclusion Risk (1-3) | Total Score      | Decision             |
|:-----------|:------------------------|:----------------|:---------------------|:-----------------|:---------------------|
| **#14**    | **No-JS Delete Safety** | **3 (Safety)**  | **3 (Systemic)**     | **6 (Critical)** | **MUST FIX (Fix 1)** |
| **#15**    | **No-JS Edit Cancel**   | **3 (Blocker)** | **3 (Systemic)**     | **6 (Critical)** | **MUST FIX (Fix 2)** |
| #16        | No-JS Search Button     | 1 (Affordance)  | 2 (Cognitive)        | 3 (Medium)       | Defer                |
| #5         | Filter Persistence      | 2 (Usability)   | 1 (General)          | 3 (Medium)       | Defer                |
| #7         | Daily Summary View      | 1 (Enhancement) | 2 (Cognitive)        | 3 (Medium)       | Defer                |

---

## 2. Selected Priorities (Rationale)

### Priority 1: No-JS Delete Safety (Fix #1)

* **The Issue**: Deleting a task in No-JS mode happens immediately via a POST request. The user is bypassed by the lack
  of a confirmation dialog (`hx-confirm` is ignored).
* **Why Critical (Score 6)**:
    * **Severity (3)**: Violates WCAG 3.3.4 (Error Prevention). Irreversible data loss causes high user anxiety (
      Confidence score: 2/5).
    * **Inclusion (3)**: Affects all users in restricted environments (No-JS), corporate proxies, or text-based
      browsers.
* **The Fix**: Implement a dedicated "Interceptor" page (`/delete/confirm`) for No-JS users.

### Priority 2: Edit "Cancel" Button Broken (Fix #2)

* **The Issue**: The "Cancel" button in the edit form is a `<button hx-get>` which is completely inert (non-functional)
  when JavaScript is disabled.
* **Why Critical (Score 6)**:
    * **Severity (3)**: Functional Blocker. The user is trapped in the edit state and cannot exit without using browser
      navigation, violating WCAG 2.1.1 (Keyboard) parity.
    * **Inclusion (3)**: Completely excludes No-JS users from the "Cancel" functionality.
* **The Fix**: Use the HTML5 `formaction` attribute to provide a robust fallback that works without JavaScript.

---

## 3. Deferred Items (Why?)

### Backlog #16: Missing Search Button (No-JS)

* **Reasoning**: While visual affordance is missing (Score 3), the functionality still works via the "Enter" key. It is
  not a blocker.
* **Plan**: Defer to Semester 2 (UI Polish phase).

### Backlog #5: Filter Persistence

* **Reasoning**: Losing search terms after reload is annoying (Usability), but it does not prevent the user from
  completing the task.
* **Plan**: Requires complex URL parameter handling; deferred due to time constraints in Lab 2.