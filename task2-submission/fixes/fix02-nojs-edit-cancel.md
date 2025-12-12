# Fix 02: Edit Cancel Button Broken in No-JS

## Problem Statement

**Evidence**:

- **Quantitative**: Task 3 (Edit) resulted in **100% Functional Failure** for the No-JS participant (unable to cancel).
- **Qualitative**: Pilot notes (P2): "I clicked cancel but it didn't do anything... is it broken?"
  **Impact**: **WCAG 2.1.1 (Keyboard)** and **Robustness** failure. Interactive controls must be operable across
  different user agents (including those without JS).
  **Priority Score**: Severity 3 (Blocker) + Inclusion Risk 3 (Systemic No-JS) = **6 (Critical)**.

## Solution

Utilized the HTML5 `formaction` attribute on the Cancel button. This overrides the form's default POST method, allowing
the button to act as a "GET" navigation link to the list view in No-JS environments, while retaining the visual styling
of a button.

### Before (Week 9 Implementation)

The button was a generic element relying solely on the `hx-get` attribute, which is ignored by the browser when
JavaScript is disabled.

```html

<div class="grid">
    <button type="submit">Save</button>

    <button
            hx-get="/tasks/{{ task.id }}/view"
            class="outline secondary">
        Cancel
    </button>
</div>
```

### After (Week 10 Redesign)

Converted to a submit button with `formaction` override.

```html

<div class="grid">
    <button type="submit" class="outline">Save</button>

    <button
            type="submit"
            class="outline"
            formaction="/tasks"
            formmethod="get"
            hx-get="/tasks/{{ task.id }}/view"
            hx-target="#task-{{ task.id }}"
            hx-swap="outerHTML push-url:false">
        Cancel
    </button>
</div>
```

### Verification

- **Manual Test**: Disabled JavaScript → Entered Edit Mode → Clicked Cancel → Verified return to List View.
- **Visual Check**: Confirmed that `Save` and `Cancel` buttons now have identical styling (both `<button>`).
- **Screenshot**: `fix-cancel-code.png`

