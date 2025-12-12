# WCAG 2.2 Level AA Compliance Audit

## 1. Critical Failures Remedied

The following criteria were identified as failures in Week 9 and have been remediated in the final submission.

### SC 3.3.4: Error Prevention (Legal, Financial, Data) (Level AA)

* **Requirement**: For Web pages that cause legal commitments or transaction data to be modified or deleted, reversible
  actions or confirmation must be provided.
* **Pre-Fix Status**: **FAILED**. In No-JS mode, the "Delete" button triggered an immediate `POST` request without
  confirmation.
* **Post-Fix Status**: **PASSED**.
* **Evidence**: Implemented an interceptor pattern. No-JS users are routed to `/tasks/{id}/delete/confirm` (See
  `fix01-nojs-delete-safety.md`).
* **Verification**: Manual test confirmed navigation to confirmation page when JS is disabled.

### SC 2.1.1: Keyboard (Level A)

* **Requirement**: All functionality of the content is operable through a keyboard interface without requiring specific
  timings for individual keystrokes.
* **Pre-Fix Status**: **FAILED**. The "Cancel" button in the Edit form (`<button hx-get>`) was inert in No-JS
  environments, trapping keyboard users in the form.
* **Post-Fix Status**: **PASSED**.
* **Evidence**: Applied `formaction` attribute to the Cancel button, ensuring it functions as a standard navigation
  control in all user agents (See `fix02-nojs-edit-cancel.md`).
* **Code Reference**:
  ```html
  <button type="submit" formaction="/tasks" formmethod="get">Cancel</button>

------

## 2. General Compliance Check (Baseline)

The following criteria were verified as compliant across the application baseline.

| **Criteria** | **Description**        | **Status** | **Notes**                                                                   |
|--------------|------------------------|------------|-----------------------------------------------------------------------------|
| **1.3.1**    | Info and Relationships | **PASS**   | Semantic HTML used (headings, lists, form labels).                          |
| **1.4.3**    | Contrast (Minimum)     | **PASS**   | Text contrast > 4.5:1 (Pico.css defaults).                                  |
| **2.4.7**    | Focus Visible          | **PASS**   | Default focus rings preserved on all interactive elements.                  |
| **3.3.1**    | Error Identification   | **PASS**   | Validation errors identified via text (Inline for HTMX, Summary for No-JS). |
| **4.1.2**    | Name, Role, Value      | **PASS**   | Standard HTML controls used; ARIA used sparingly and correctly.             |

## 3. Remaining Issues (Backlog)

- **SC 2.4.3 (Focus Order)**: After deleting a task via HTMX, focus is currently reset to the `body` rather than the
  adjacent task or "Add" button. (Deferred to Semester 2).