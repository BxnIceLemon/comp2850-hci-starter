# Redesign Plan — Week 10

**Goal**: Address Critical (Priority 1) and High (Priority 2) issues identified in Week 9 Peer Pilots.

---

## Fix 1: No-JS Delete Safety (Priority 1)

### Problem

In Week 9, the "Delete" button performed an immediate `POST` request. In No-JS mode, this skipped the browser's
confirmation dialog (`hx-confirm`), causing user anxiety and violating **WCAG 3.3.4 (Error Prevention)**.

### Solution Pattern: "Interceptor Route"

We implement a dedicated confirmation page for No-JS users.

1. **HTMX Flow**: Remains unchanged (intercepts click, shows popup, deletes inline).
2. **No-JS Flow**: The "Delete" button becomes a link to a new confirmation route.

### Technical Implementation

* **New Route**: `GET /tasks/{id}/delete/confirm` rendering `tasks/delete_confirm.peb`.
* **Template Change**:
    ```html
    <form action="/tasks/1/delete" method="post"><button>Delete</button></form>

    <a href="/tasks/1/delete/confirm"
       hx-delete="/tasks/1"
       hx-confirm="Are you sure?">
       Delete
    </a>
    ```

---

## Fix 2: Robust Edit Cancel Button (Priority 2)

### Problem

The "Cancel" button in the edit form was implemented as `<button hx-get="...">`. In No-JS mode, this button was inert (
dead), trapping the user in the edit state. This violated **WCAG 2.1.1 (Keyboard)** functionality parity.

### Solution Pattern: "Form Action Override"

We initially considered changing the tag to `<a>`, but that caused styling inconsistencies with the "Save" button.
**Final Decision**: We use the HTML5 `formaction` attribute on a submit button. This allows the button to serve as a "
GET" navigation link in No-JS mode while keeping perfect visual parity with the Save button.

### Technical Implementation

* **Attribute Strategy**: Use `formaction="/tasks"` and `formmethod="get"` to override the form's default POST behavior.
* **Template Change**:
    ```html
    <button hx-get="...">Cancel</button>

    <button type="submit"
            formaction="/tasks"
            formmethod="get"
            hx-get="/tasks/1/view"
            class="outline">
            Cancel
    </button>
    ```
* **Trade-off**: In No-JS mode, clicking Cancel sends query parameters (e.g., `?title=...`) to the list view. This is
  acceptable as the server ignores them, and it ensures robust navigation without JavaScript.

---

## Deferred Items (Priority 3)

### Filter Search Button

* **Issue**: No visual "Search" button for No-JS users (relying on Enter key).
* **Decision**: Deferred. While an affordance issue, it is not a safety risk or functional blocker.

### Filter Persistence

* **Issue**: Search terms are lost after page reload.
* **Decision**: Deferred to Semester 2 (requires session management or URL param refactoring).