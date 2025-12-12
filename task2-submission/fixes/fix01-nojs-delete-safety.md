# Fix 01: No-JS Delete Safety (Missing Confirmation)

## Problem Statement

**Evidence**:

- **Quantitative**: P2 (No-JS) confidence rating dropped to **2/5** (Gap: -3.0 vs HTMX). Server logs showed deletion
  time of **1.0 ms** (Median), indicating the confirmation dialog was skipped entirely.
- **Qualitative**: Pilot notes (P2): "Wait, did I delete it? It didn't ask me..."
  **Impact**: **WCAG 3.3.4 (Error Prevention)** failure. For data deletion, reversible actions or confirmation are
  mandatory.
  **Priority Score**: Severity 3 (Safety) + Inclusion Risk 3 (Systemic No-JS) = **6 (Critical)**.

## Solution

Implemented an "Interceptor Route" pattern. For No-JS users, the delete action now navigates to a dedicated confirmation
page instead of submitting an immediate POST request.

### Before (Week 9 Implementation)

The button relied on `hx-confirm` (JavaScript) for safety. Without JS, it fell back to a raw form submission.

```html

<div class="grid">
    <form action="/tasks/{{ task.id }}/delete" method="post">
        <button class="outline contrast">Delete</button>
    </form>
</div>
```

### After (Week 10 Redesign)

Used a progressive enhancement link (`<a>`) that navigates to a confirmation page in No-JS mode, but upgrades to an HTMX
AJAX delete when JS is available.

```html

<div class="grid">
    <a
            href="/tasks/{{ task.id }}/delete/confirm"
            role="button"
            class="outline contrast"
            hx-delete="/tasks/{{ task.id }}"
            hx-confirm="Are you sure you want to delete '{{ task.title }}'?"
            hx-target="#task-{{ task.id }}"
            hx-swap="delete">
        Delete
    </a>
</div>
```

### Verification

- **Manual Test**: Disabled JavaScript → Clicked Delete → Verified navigation to `/tasks/{id}/delete/confirm`.
- **Screenshot**: `fix-delete-confirm-nojs.png`