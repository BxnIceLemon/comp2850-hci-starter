# Prototyping Constraints & Trade-offs

## Rendering splits
- Full page: `/tasks` returns layout + list + pager.
- Fragment: `/tasks/fragment` returns list + pager + OOB status.

## URL & History
- `hx-push-url="true"` on filter and pager links keeps Back/Forward meaningful.

## Accessibility hooks
- Live region `#status` announces changes.
- Result count associated with list via `aria-describedby`.

## Performance notes
- Page size: 5 items; consider server time vs client cost.

## Future risks
- Template duplication between full page and fragments.
- Focus management after deletes (ensure next focusable target).

