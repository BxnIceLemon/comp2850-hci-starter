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

---

# Prototyping Constraints and Trade-offs — Week 8

## Dual-Path Architecture

### Design Decision

Every route handles both HTMX (enhanced) and no-JS (baseline) requests.

### Benefits

- **Inclusion**: Works for everyone regardless of JS availability.
- **Resilience**: Graceful degradation if JS fails to load or execution is blocked.
- **Testing**: Baseline path proves server-first architecture is sound.
- **Progressive enhancement**: Start with accessible baseline, enhance with HTMX.

### Costs

- **Code complexity**: Each route has conditional logic (`if (call.isHtmx())`).
- **Response duplication**: Must generate both fragments (for OOB/swaps) and full pages (for redirects).
- **Testing burden**: Every feature requires two test paths (verified via `scripts/nojs-check.md`).
- **Performance**: No-JS path triggers full page reloads (slower perceived performance).

### Risks

- **Divergence**: HTMX and no-JS paths could drift if not tested regularly.
- **Error handling**: Easy to forget no-JS error path (redirects) during rapid development.
- **Template maintenance**: Changes to page structure must update both full templates and fragments.

### Mitigations

- ✅ **Verification script**: `scripts/nojs-check.md` provides repeatable tests.
- ✅ **Shared partials**: `_item.peb`, `_list.peb` used by both paths (single source of truth).
- ✅ **Header control**: Used `HX-Reswap: none` header to prevent list disappearance during validation errors.
- ✅ **Backlog tracking**: Log parity issues immediately (see `backlog/backlog.csv`).
- 🔮 **Future**: Automated Playwright tests running in a disabled-JS context.

---

## Validation Strategy

### Design Decision

All validation on server. No client-side validation logic (HTML5 `required` attribute used for accessibility, but logic
enforced by backend).

### Benefits

- **Security**: Client-side validation can be bypassed (view source, disable JS, curl).
- **Consistency**: Same validation rules for HTMX and no-JS paths.
- **Accessibility**: Server returns appropriate error format for each context (Inline for HTMX, Summary for No-JS).

### Costs

- **Latency**: Must wait for server round-trip to see validation errors.
- **UX**: No instant feedback on typos (could add client-side hints later).

### Risks

- **Frustration**: People might repeatedly submit invalid forms before reading error.
- **Network dependency**: People offline can't get any feedback.

### Mitigations

- ✅ **Clear error messages**: Specific, actionable text ("Title is required").
- ✅ **Accessible error identification**: WCAG 3.3.1 compliance (`aria-invalid`, `aria-describedby`, `role=alert`).
- ✅ **Context-aware display**: HTMX updates specific input fields (inline), while No-JS renders a top-level Error
  Summary.
- 🔮 **Future enhancement**: Add client-side hints (maxlength indicator, real-time char count) as progressive
  enhancement.

---

## Delete Confirmation

### Design Decision

HTMX path uses `hx-confirm` (browser confirmation dialog). No-JS path has no confirmation (direct POST).

### Benefits

- **HTMX**: Prevents accidental deletions for people with JavaScript enabled.
- **Implementation simplicity**: No intermediate confirmation page needed.

### Costs

- **Inconsistency**: Different UX depending on JS availability.
- **Accessibility**: Browser confirm dialogs are not customisable (cannot improve copy or button labels).

### Risks

- **Accidental deletion**: People without JavaScript might delete tasks by mistake.
- **Compliance**: Irreversible actions typically require confirmation (WCAG 3.3.4 Error Prevention).

### Mitigations

- ✅ **Documentation**: Trade-off explicitly noted in constraints doc.
- ✅ **Semantic Fallback**: Uses `DELETE` method for HTMX and `POST` for No-JS (via `<form>`).
- ⚠️ **Research with participants** (Week 9): Test whether delete accidents occur in pilots.
- 🔮 **Future option**: Add "Undo" feature (restore from soft-delete within 30s) to mitigate lack of confirmation.
- 🔮 **Future option**: Implement a dedicated `/tasks/{id}/delete/confirm` page for No-JS users.

---

## State Management

### Design Decision

Use query parameters for filter and page state (`?q=search&page=2`).

### Benefits

- **Shareable**: URL captures full state (can bookmark or share filtered view).
- **History**: Back/forward buttons work predictably (tested via PRG pattern).
- **No-JS compatible**: Query params work natively with HTML forms.
- **Stateless server**: No session state needed for pagination.

### Costs

- **URL pollution**: Long query strings for complex filters.
- **Analytics**: Harder to track "unique pages" if many query variations exist.

### Risks

- **Drift**: If fragment requests use different query params than full page, state can desync.
- **History Stack**: Rapid filtering can pollute browser history if not handled carefully (mitigated via `hx-push-url`).

### Mitigations

- ✅ **Consistent param names**: Use `q` and `page` everywhere.
- ✅ **Hidden input reset**: Search form includes `<input type="hidden" name="page" value="1">` to ensure filtering
  always resets pagination in URL.
- ✅ **Encoding**: Use `call.request.queryParameters` (Ktor handles encoding).
- 🔮 **Future**: If filters grow complex, consider POST-redirect pattern or session-based filter state.

---

## Performance Considerations

### Active Search Debounce

- **Decision**: 300ms debounce on `hx-trigger="keyup changed delay:300ms"`.
- **Benefit**: Reduces server load (doesn't fire on every keystroke).
- **Cost**: 300ms perceived latency before filter applies.
- **Mitigation**: Could show loading indicator (`hx-indicator`) during request.

### Page Size

- **Decision**: 10 tasks per page (Hardcoded in Routes).
- **Benefit**: Fast page loads, manageable scroll.
- **Cost**: More pagination clicks for large datasets.
- **Mitigation**: Pagination UI allows standard Next/Previous navigation.

### Fragment Size

- **Decision**: Return `_list + _pager + status` (~2-5KB) instead of full page (~15KB) for HTMX requests.
- **Benefit**: Significant bandwidth reduction on filter/pagination.
- **Cost**: Requires dual-path rendering logic in `TaskRoutes.kt`.

---

## Evidence and Testing

**Verification scripts**: `wk08/lab-w8/scripts/nojs-check.md`
**Evidence folder**: `evidence/wk8/nojs-parity/`
**Backlog references**: IDs `wk8-01` to `wk8-XX`

**Review schedule**: Re-run parity tests after:

- Any route changes
- Template structure updates
- Before Week 9 instrumentation (ensure baseline is solid)

**Ownership**: Entire team responsible for verifying parity. Pair on changes: one person tests HTMX, another tests
no-JS.
