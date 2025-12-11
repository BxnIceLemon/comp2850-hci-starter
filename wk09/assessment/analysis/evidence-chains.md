# Evidence Chains — Week 9

## Chain 1: Lack of Delete Confirmation (No-JS)

**Raw data**:

- **Quantitative**: `metrics_P2.csv` (No-JS) shows Task 3 (Delete) duration was only **1.0s** (vs HTMX ~2.3s),
  indicating no interaction with a dialog.
- **Qualitative**: P2 (No-JS) Confidence **2/5**. Quote: "Wait, did I delete it? It didn't ask me 'are you sure?'"

**Finding**: No-JS participants experience anxiety and surprise because the deletion happens immediately (POST request)
without a confirmation step, unlike the HTMX path which has a popup.

**Backlog item**: `wk9-04`, "Delete confirmation missing in no-JS mode", Severity: High, Inclusion risk: Cognitive (
Safety), Error Prevention.

**WCAG**: **3.3.4 Error Prevention (Legal, Financial, Data)** - For data deletion, reversible actions or confirmation
are required.

**Evidence file**: `wk09/data/pilot-notes.md` (Section P02, Task 3)

**Screenshot**: `wk09/evidence/nojs-delete-surprise.png` (Optional: Screenshot of the list with the item gone instantly)

---

## Chain 2: Broken "Cancel" Button in Edit Mode (No-JS)

**Raw data**:

- **Qualitative**: P2 (No-JS) Observation during Task 4: "I clicked cancel but it didn't do anything."
- **Context**: The button relies on `hx-get` and lacks a standard `href` fallback, rendering it inert when JS is
  disabled.

**Finding**: The "Cancel" button in the Edit form is non-functional for users without JavaScript, trapping them in the
edit state unless they manually use the browser's Back button.

**Backlog item**: `wk9-05`, "Edit form Cancel button non-functional in No-JS", Severity: High (Broken Functionality).

**WCAG**: **2.1.1 Keyboard** (Functionality must be available) & **Robustness** (Content must work across different user
agents).

**Evidence file**: `wk09/data/pilot-notes.md` (Section P02, Task 4)

**Screenshot**: `wk09/evidence/nojs-broken-cancel.png` (Screenshot of the button code or the state)

---

## Chain 3: Effective Error Recovery (Accessibility Win)

**Raw data**:

- **Quantitative**: `metrics_P2.csv` shows `validation_error` followed by `success`.
- **Qualitative**: P2 (No-JS) Quote: "Oh, there's a big red box at the top... Clicked the link, focus jumped to input."
  P3 (Keyboard) observed status message injection.

**Finding**: The dual-path validation strategy is effective. HTMX users benefit from inline context, while No-JS users
are successfully supported by the Error Summary pattern which manages focus correctly.

**Conclusion**: Current validation design meets accessibility standards; no redesign needed for this feature.

**WCAG**: **3.3.1 Error Identification** & **3.2.1 On Focus** (Focus management).

**Evidence file**: `wk09/data/pilot-notes.md` (Section P02 & P03, Task 2)

