# Assessment Draft Outline

## Section 1: Redesign Rationale (20%)

### 1.1 Priority Selection

- **Methodology**: Used Inclusion-Severity Matrix. Prioritised issues that were "Functional Blockers" (High Severity)
  or "Safety Risks" (High Inclusion Risk).
- **Selection**:
  1.  **Delete Safety (ID 14)**: Selected because P2 (No-JS) experienced "immediate deletion panic" (Safety).
  2.  **Cancel Functionality (ID 15)**: Selected because P2 was trapped in edit mode (Functional Blocker).
- **Reference**: Link to `wk10/analysis/priorities.md`.

### 1.2 Evidence from Week 9

- **Quantitative**:
    - Task 3 (Edit): **100% Error Rate** in No-JS mode (P2 failed to cancel).
    - Task 4 (Delete): **Confidence score dropped to 2/5** for No-JS user despite successful deletion.
- **Qualitative**:
    - Quote: "Wait, did I delete it? It didn't ask me..." (P2).
    - Quote: "I clicked cancel but it didn't do anything... is it broken?" (P2).
- **WCAG Failures**:
    - **WCAG 3.3.4 (Error Prevention)**: Reversible actions or confirmation required for data deletion.
    - **WCAG 2.1.1 (Keyboard)**: All functionality must be operable via keyboard (Cancel button was inert).

-----

## Section 2: Implementation (30%)

### 2.1 Fix 1: No-JS Delete Confirmation (Priority 1)

- **Problem**: Immediate `POST` request on delete caused anxiety.
- **Solution**: Implemented a dedicated confirmation page (`delete_confirm.peb`) and updated the Delete button to use
  Progressive Enhancement.
- **Code Snippet (Before)**:
  ```html
  <form action="..." method="post"><button>Delete</button></form>
  ```
- **Code Snippet (After)**:
  ```html
  <a href="/tasks/.../confirm" hx-delete="..." hx-confirm="...">Delete</a>
  ```
- **Screenshot Comparison**: `nojs-delete-surprise.png` vs `fix-delete-confirm-nojs.png`.
- **WCAG Impact**: Meets **3.3.4 (AA)** by forcing a confirmation step for non-enhanced users.

### 2.2 Fix 2: Robust Edit Cancel Button (Priority 2)

- **Problem**: `<button hx-get>` relied solely on JS, breaking the flow for No-JS users.
- **Solution**: Utilized HTML5 `formaction` attribute to handle No-JS fallback within the same button element.
- **Code Snippet (Before)**:
  ```html
  <button hx-get="...">Cancel</button>
  ```
- **Code Snippet (After)**:
  ```html
  <button type="submit" formaction="/tasks" formmethod="get" hx-get="...">Cancel</button>
  ```
- **Visual Consistency**: Achieved 100% visual parity between "Save" and "Cancel" (both are `<button>` tags now).
- **WCAG Impact**: Meets **2.1.1 (A)** by ensuring the control performs an action (navigation) regardless of technology
  stack.

### 2.3 Trade-Offs

- **Performance**: No-JS delete flow now requires 2 round-trips (Get Page -\> Post Delete) instead of 1. **Acceptable**
  because safety \> speed.
- **Cleanliness**: The `formaction` approach on Cancel sends dirty query parameters (e.g., `?title=...`) to the list
  view. **Acceptable** because the server ignores them, and it avoids complex JavaScript logic.

-----

## Section 3: Verification (30%)

### 3.1 Re-Testing

- **Manual Verification (No-JS)**:
    - Confirmed clicking "Delete" leads to `/confirm` page.
    - Confirmed clicking "Cancel" in Edit mode returns to List view.
- **Regression Testing (HTMX)**:
    - Confirmed HTMX still intercepts clicks (Inline Edit and Confirm Dialog still work).
    - Verified visuals (Edit/Delete buttons aligned in list; Save/Cancel buttons aligned in form).

### 3.2 Accessibility Impact

- **Cognitive**: Reduced anxiety for users who fear accidental data loss (Confirmation Page).
- **Robustness**: Application now functions fully on text-based browsers (Lynx) or environments where JS is blocked (
  Corporate security).

-----

## Section 4: Reflection (20%)

### 4.1 Process Critique

- **Success**: The "Dual-Path" architecture proved resilient. We fixed No-JS issues without breaking the HTMX
  experience.
- **Challenge**: CSS styling consistency between `<a>` and `<button>` tags was difficult.
    - *Insight*: Ultimately solved by switching Cancel back to a `<button>` with `formaction` rather than forcing it to
      be an `<a>`.
- **Time Management**: Prioritisation Matrix helped us drop the "Filter Search Button" issue to focus on critical safety
  bugs.

### 4.2 Future Work

- **Deferred**: "Filter Persistence" (keeping search terms after reload) remains a usability friction point.
- **Next Step**: Implement "Undo Delete" (Soft Delete) in Semester 2 to further enhance error prevention (WCAG 3.3.4
  Level AAA).

-----

## Files to Include

- `wk10/analysis/priorities.md`
- `wk10/design/redesign-plan.md`
- `wk10/evidence/fix-delete-confirm-nojs.png`
- `wk10/evidence/fix-edit-buttons-consistent.png`
- Updated `backlog/backlog.csv`
