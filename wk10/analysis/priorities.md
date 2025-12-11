- # Redesign Priorities — Week 10 Lab 2

  **Rationale**: Priorities are determined by **Severity** (Functional blocker / Safety risk) and **Inclusion Risk** (
  Who is excluded?).

  ## Priority 1: No-JS Delete Safety (MUST FIX)

  Issue: Deleting a task in No-JS mode happens immediately (POST request) without any confirmation dialog, causing user
  anxiety and safety risks.

  Evidence:

    - **Quantitative**: P2 (No-JS) Confidence rating **2/5**.

    - Qualitative: P2 quote "Wait, did I delete it? It didn't ask me...".

      WCAG: 3.3.4 Error Prevention (Legal, Financial, Data) (Level AA).

      Fix: Implement a dedicated confirmation page (GET /tasks/{id}/delete/confirm) for No-JS users.

      Effort: 1.5 hours (New route + New template).

  ## Priority 2: Edit "Cancel" Button Broken (MUST FIX)

  Issue: The "Cancel" button in the edit form is a \<button hx-get\> which is completely inert (non-functional) when
  JavaScript is disabled. Users are trapped in edit mode.

  Evidence:

    - **Quantitative**: P2 (No-JS) Error Rate **100%** on Task 3 (Edit).

    - Qualitative: P2 observation "Clicked cancel but nothing happened".

      WCAG: 2.1.1 Keyboard (Level A) - Functionality must be operable.

      Fix: Convert the button to a semantic link (\<a href="/tasks"\>) that progressively enhances to HTMX.

      Effort: 20 minutes (Template change only).

  ## Deferred (Post-Assessment or Semester 2)

    - **Filter Persistence**: Preserving search terms in the URL after reloading (Usability).
    - **Undo Delete**: Complex state management required (Enhancement).

