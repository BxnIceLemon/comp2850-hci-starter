# Week 9 Evaluation Planning Sketch

## Events to Log

1. **TaskCreated**

   * Fields: `task_id`, `timestamp`, `is_critical` (bool), `source_view` (e.g. “today-summary”, “full-list”), `input_method` (`keyboard`, `mouse`, `touch`).
   * Purpose: Measure time-on-task and whether high-priority tasks are being captured correctly.

2. **TaskDeleted**

   * Fields: `task_id`, `timestamp`, `input_method`, `confirmation_shown` (bool), `undo_available` (bool).
   * Purpose: Check that delete is reachable with keyboard-only (S1), and that users see confirmation/undo (S2, error recovery).

3. **FilterChanged / FilterRestored**

   * `FilterChanged`: `timestamp`, `filter_type` (module, priority, date), `filter_value`, `view` (desktop/mobile).
   * `FilterRestored`: `timestamp`, `filter_type`, `filter_value`, `was_restored_on_reload` (bool).
   * Purpose: Track how often users rely on filters and whether state is preserved across PRG reloads (S5: maintain context in long lists).

4. **ValidationError**

   * Fields: `timestamp`, `field_name` (e.g. `date`), `error_type` (past_date, missing_required, invalid_format), `view_context` (create/edit), `is_critical_task` (bool).
   * Purpose: Monitor date/time errors and confirm improved validation catches past or invalid dates (S4 / WCAG 3.3.1).

5. **StatusMessageShown**

   * Fields: `timestamp`, `message_type` (success, error, info), `action` (create, delete, update), `delivery_channel` (visible_only, SR_live_region), `context` (JS, no_JS).
   * Purpose: Verify that confirmation feedback is present and exposed to screen readers (S2 / WCAG 4.1.3).

6. **ModeSwitch / SummaryViewed**

   * `ModeSwitch`: `timestamp`, `from_mode` (full, travel), `to_mode`.
   * `SummaryViewed`: `timestamp`, `summary_type` (today_reassurance, travel_summary), `tasks_today`, `tasks_done`.
   * Purpose: Evaluate use of “travel mode” and “daily reassurance” screens (S6), and see if people rely on summaries rather than long, overwhelming lists.


---

## Metrics to Capture

1. **Time-on-task (per scenario)**

   * Definition: `TaskDeleted.timestamp - scenario_start` for delete, `TaskCreated.timestamp - scenario_start` for add, etc.
   * Related stories: S1 (keyboard access), S2 (confirmation), S4 (validation), S5 (filter context).

2. **Error rate (validation & accidental actions)**

   * Validation error rate: `#ValidationError / #TaskCreated or #TaskUpdated`.
   * Past-date error rate: `#ValidationError[field=date & error_type=past_date] / #date_inputs`.
   * Accidental delete rate: `#TaskDeleted where undo_used=true / #TaskDeleted`.
   * Related stories: S4 (safe input), S2 (clear feedback when correcting errors).

3. **Completion rate (per test scenario)**

   * Definition: `% of participants who complete the scripted task (add/delete/filter) without moderator intervention`.
   * Related stories: S1 (delete via keyboard-only), S5 (filter + reload), S6 (finding information in summary views).

4. **Context persistence & filter stability**

   * Filter restoration rate: `#FilterRestored[was_restored_on_reload=true] / #FilterChanged`.
   * “Context lost” incidents: count of times participants verbally report “I lost where I was” or have to re-apply filters.
   * Related stories: S5 (maintain context in long lists).

5. **Subjective confidence and reassurance**

   * Post-task ratings (Likert 1–5) for:

     * “I felt confident that my actions were saved correctly.”
     * “I felt in control and not overwhelmed by the list.”
   * Related stories: S2 (trust & feedback), S6 (reassuring daily summary), C’s anxiety about deadlines.


---

## Test Scenarios

### Scenario 1: Add and confirm a critical task with date validation

**Goal:** Test S2 (confirmation feedback) + S4 (error messages) + partial S6 (reassuring overview).

* **Setup:** Participant is told they have an important doctor’s appointment next week.
* **Task:**

  1. Add a new task “Doctor appointment” marked as critical.
  2. First, intentionally enter a **past date** (researcher prompts or design scenario so that many will do this).
  3. Correct the error and save with the correct date.
  4. Check the daily summary / reassurance view to verify the task is listed.
* **Success criteria:**

  * Participant notices and understands the validation error.
  * Participant can fix the date and save without moderator help.
  * Participant receives a clear status message and can later see the appointment in a summary view.
* **Events & metrics used:**

  * `ValidationError` (error rate, type), `TaskCreated`, `StatusMessageShown`.
  * Time-on-task, error rate, completion rate, confidence rating.

---

### Scenario 2: Delete a task with keyboard only

**Goal:** Test S1 (keyboard access) + S2 (confirmation feedback).

* **Setup:** A list of tasks is pre-populated, with one clearly labeled “Delete this task in the study”.
* **Task:**

  1. Using **keyboard only** (no mouse), navigate to the target task.
  2. Delete it.
  3. Confirm, using only what the interface shows/tells you, that it is gone.
* **Success criteria:**

  * Participant can reach the delete control and activate it via keyboard alone.
  * Focus never “disappears” (participant always knows where they are).
  * A success status message is displayed and announced (if SR is used).
* **Events & metrics used:**

  * `TaskDeleted` (input_method=keyboard), `StatusMessageShown`.
  * Time-on-task, completion rate, keyboard-only success rate.

---

### Scenario 3: Filter tasks, reload, and maintain context (desktop or laptop)

**Goal:** Test S5 (maintaining context in long lists) + partly S6 (using summaries to reduce overload).

* **Setup:** Task list contains multiple modules (e.g. COMP2850, COMP2870, others) and priorities.
* **Task:**

  1. Filter the list to show only tasks for a specific module (e.g. COMP2850) or “critical” tasks.
  2. Find a given target task in that filtered view.
  3. Reload the page (F5 or similar).
  4. Confirm whether the filter is still applied and you can still see the same subset.
* **Success criteria:**

  * After reload, the participant’s filtered context is preserved OR they immediately understand how to get back to that filtered view.
  * Participant does not express feeling “lost” or forced to rebuild context from scratch.
* **Events & metrics used:**

  * `FilterChanged`, `FilterRestored`.
  * Filter restoration rate, time-on-task, self-reported cognitive load / confusion.

