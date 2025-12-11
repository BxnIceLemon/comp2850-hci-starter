# Evaluation Tasks — Week 9 (Customized)

## Task 1: Find and Count (Filter)

**Scenario**: "You have a long list of tasks and need to check if you remembered to add the 'Party' related items. Find
all tasks containing the word 'Party' and tell me how many there are."

**Setup**:

- Ensure database has at least 15 tasks.
- Include 3 tasks with "Party" in the title (e.g., "Buy Party snacks", "Party invite list", "Clean up after party").

**Success criteria**:

- Participant uses the filter input (types "Party").
- Filter applies successfully (HTMX updates list or No-JS reloads).
- Participant correctly identifies there are 3 tasks.
- **Key Observation**: Did they notice the "Showing 3 tasks" text (Accessibility check)?

**Inclusion focus**: Screen reader result announcement, Keyboard navigation to filter.

---

## Task 2: Validation Stress Test (Add Task)

**Scenario**: "You are in a rush and try to quickly add a new task for 'Call Mom', but you accidentally hit Enter before
typing anything. Observe what happens, then correct your mistake and add the task properly."

**Instructions**:

1. Try to submit an empty form.
2. Read the error message.
3. Successfully add the task "Call Mom".

**Success criteria**:

- Error message appears clearly (HTMX: inline below input; No-JS: summary at top).
- Participant recovers from error without refreshing manually.
- Task is eventually added to the list.

**Inclusion focus**: Error identification (WCAG 3.3.1), Focus management (does focus go to input?).

---

## Task 3: Safe Deletion (Delete)

**Scenario**: "The task 'Buy old newspapers' is no longer needed. Please delete it. Be careful not to delete anything
else."

**Setup**:

- A task named "Buy old newspapers" exists in the list.

**Success criteria**:

- Participant clicks Delete.
- **HTMX Mode**: Confirm dialog appears, participant clicks OK.
- **No-JS Mode**: Task deletes immediately (observe if they seem surprised by lack of confirm).
- Task disappears from UI.

**Inclusion focus**: Confirmation dialog accessibility, Status message ("Task deleted").

---

## Task 4: Fix a Typo (Edit)

**Scenario**: "You noticed a typo. The task 'By milk' should be 'Buy milk'. Fix the spelling."

**Instructions**:

1. Locate "By milk".
2. Change it to "Buy milk".
3. Save the change.

**Success criteria**:

- Edit form opens inline.
- Save works.
- Updated title is visible immediately.

**Inclusion focus**: Tab order inside the edit form, Cancel button functionality.