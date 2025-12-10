# Prototyping Constraints & Trade-offs

## Rendering splits
- **Full page**: `GET /tasks` returns the complete HTML document (layout + list + pager). This ensures the initial load works and provides a fallback for No-JS users.
- **Fragment**: `GET /tasks/fragment` returns only the `_list.peb` and `_pager.peb` partials, plus an OOB (Out-of-Band) status update. This reduces payload size for HTMX interactions (search/pagination).

## URL & History
- `hx-push-url="true"` is applied to filter form and pagination links.
- **Benefit**: Keeps the browser history stack meaningful. Users can use the Back/Forward buttons to navigate through search states.
- **Benefit**: Search results and page states are bookmarkable (e.g., `/tasks?q=milk&page=2`).

## Accessibility hooks
- **Live Region**: A `<div id="status" aria-live="polite">` is updated via HTMX OOB swaps to announce dynamic changes (e.g., "Found 5 tasks", "Task added") to screen readers without interrupting the user.
- **Result Count**: The task list `<ul>` is associated with a hidden result count summary via `aria-describedby`, ensuring users know how many items are currently displayed.
- **Skip Link**: Positioned off-screen (`left: -9999px`) to be visually hidden but accessible via keyboard focus, complying with WCAG 2.4.1.

## Performance & Persistence
- **Page size**: Configured to 10 items (hardcoded in `TaskRoutes.kt`) to balance server load and client rendering cost.
- **Persistence**: Implemented `TaskStore` using a simple CSV file (`data/tasks.csv`).
    - *Constraint*: Not suitable for high-concurrency production environments but sufficient for a single-user prototype.
    - *Trade-off*: Easy to inspect/debug data manually vs. lack of advanced query capabilities.

## Future risks
- **Template duplication**: While partials reduce this, logic for passing context (like `editingId` or `error`) must be maintained in both full-page and fragment routes.
- **Focus management**: After deleting a task, focus should ideally move to the next logical item. Currently, it may be lost or reset to the body depending on the browser.