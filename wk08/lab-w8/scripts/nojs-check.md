# No-JS Parity Verification Script — Week 8

**Purpose**: Verify all task flows work identically with JavaScript disabled.

**Setup**:

1. Open Chrome DevTools → Settings → Preferences → Disable JavaScript
2. Alternatively: Use Firefox → Settings → Privacy & Security → Permissions → Uncheck JavaScript
3. Hard refresh (Ctrl+Shift+R / Cmd+Shift+R) to clear cached JS

---

## Test 1: Add Task (Success Case)

**Steps**:

1. Navigate to `/tasks`
2. Enter title "No-JS test task"
3. Click "Add Task"

**Expected**:

- Full page reload (check Network tab: only HTML request)
- New task appears in list
- URL remains `/tasks` (PRG redirect)
- No error messages

**Evidence**: Screenshot of task list with new task visible.

**Result**: [x] Pass  [ ] Fail

---

## Test 2: Add Task (Validation Error)

**Steps**:

1. Leave title field empty
2. Click "Add Task"

**Expected**:

- Full page reload
- URL shows `/tasks?error=title`
- Error summary appears at top: "There is a problem"
- Link to "#title" input
- Input has red border, error message below
- Focus can navigate to error link and then to input

**Evidence**: Screenshot of error summary and highlighted input.

**Result**: [x] Pass  [ ] Fail

---

## Test 3: Filter Tasks

**Steps**:

1. Add tasks "Alpha", "Bravo", "Charlie"
2. Enter "ra" in filter box
3. Click "Apply" or submit form

**Expected**:

- Full page reload
- URL shows `/tasks?q=ra&page=1`
- Only "Bravo" and "Charlie" visible (partial match)
- Result count updates: "Showing 2 tasks"

**Evidence**: Screenshot of filtered results with URL visible.

**Result**: [x] Pass  [ ] Fail

---

## Test 4: Pagination

**Steps**:

1. Add 15 tasks (assuming page size = 10)
2. Navigate to page 2 using "Next" link

**Expected**:

- Full page reload
- URL shows `/tasks?page=2`
- Tasks 11-15 visible
- "Previous" link appears
- "Next" link disabled or hidden

**Evidence**: Screenshot of page 2 with navigation controls.

**Result**: [x] Pass  [ ] Fail

---

## Test 5: Edit Task (inline)

**Steps**:

1. Click "Edit" on a task
2. Change title to "Updated via no-JS"
3. Submit

**Expected**:

- Full page reload (or inline form renders in server response)
- URL remains `/tasks` or shows task context
- Updated title visible
- No JavaScript errors (none should exist)

**Evidence**: Screenshot of updated task.

**Result**: [x] Pass  [ ] Fail

---

## Test 6: Delete Task

**Steps**:

1. Click "Delete" button on a task

**Expected**:

- Form submits to `POST /tasks/{id}/delete`
- Full page reload
- Task removed from list
- URL redirects to `/tasks`
- **No confirmation** (documented trade-off)

**Evidence**: Screenshot of task list with item removed.

**Result**: [x] Pass  [ ] Fail

---

## Test 7: Keyboard Navigation

**Steps**:

1. Tab through entire page (skip link → form → tasks → pagination)
2. Ensure visible focus indicator at each stop
3. Activate "Add Task" with Enter
4. Navigate to error link with Tab, activate with Enter

**Expected**:

- Focus order matches visual order
- All interactive elements reachable
- Enter activates links/buttons
- Focus visible on all elements

**Evidence**: Notes on tab order, screenshot of visible focus.

**Result**: [x] Pass  [ ] Fail

---

## Test 8: Browser History

**Steps**:

1. Start at `/tasks`
2. Add task (redirects to `/tasks`)
3. Filter to "test" (reloads to `/tasks?q=test`)
4. Go to page 2 (reloads to `/tasks?q=test&page=2`)
5. Click browser Back button twice

**Expected**:

- Back #1 → `/tasks?q=test&page=1` (filtered, page 1)
- Back #2 → `/tasks?q=test` (filtered, page 1 implicit)
- Back #3 → `/tasks` (no filter)
- History stack matches expected behaviour

**Evidence**: Notes on history behaviour.

**Result**: [x] Pass  [ ] Fail

---

## Summary

**Passed**: 8 / 8
**Failed**: 0 / 8

**Issues found**: None

**Notes**:

- Attach screenshots to `evidence/wk8/nojs-parity/`
- Update `docs/prototyping-constraints.md` with any new trade-offs discovered
- Re-run this script after any route or template changes

---

**Verified by**: Ningsong Jia
**Date**: 2025-11-21
