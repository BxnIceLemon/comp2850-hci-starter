# Assessment Draft Outline — Week 9

## Section 1: Evaluation Plan

### 1.1 Test Tasks

- **Source**: `wk09/research/tasks.md`
- **Tasks Designed**:
    1. **Task 1 (Filter)**: Find tasks containing "Party" and count them. (Focus: Search efficiency & screen reader
       announcements).
    2. **Task 2 (Add - Validation)**: Attempt to add a task with a blank title. (Focus: Error recovery & focus
       management).
    3. **Task 3 (Edit)**: Fix a typo in "By milk". (Focus: Inline editing & interaction flow).
    4. **Task 4 (Delete)**: Delete "Old newspapers". (Focus: Safety & confirmation dialogs vs. direct deletion).
- **Inclusion Focus**: Designed to stress-test No-JS fallbacks and Keyboard navigation (Tab order).

### 1.2 Metrics

- **Source**: `wk09/research/measures.md`
- **Quantitative Metrics**:
    - **Time-on-Task (Server)**: Measured via `call.timed` in `metrics.csv` to isolate system performance from network
      latency.
    - **Success Rate**: Binary (Pass/Fail).
    - **Error Rate**: Specifically tracking validation errors in Task 2.
- **Qualitative Metrics**:
    - **Confidence Score (1-5)**: Self-reported after each task to measure psychological safety (critical for Delete
      task).
    - **Think-Aloud**: Recorded verbal frustrations (e.g., "Wait, did I delete it?").
- **Justification**: Server-side timing avoids client performance bias; Confidence scores capture "emotional safety"
  which speed metrics miss.

### 1.3 Protocol

- **Source**: `wk09/research/protocol.md`
- **Ethical Compliance**:
    - **Consent**: Verbal consent obtained before session; participants informed of "Right to Withdraw".
    - **Privacy**: Anonymous Session IDs (e.g., `P1_e06zfc`) used in logs; no PII collected in compliance with UK GDPR.
- **Procedure**: Counter-balanced testing (HTMX first vs. No-JS first) to reduce learning effects.

------

## Section 2: Findings

### 2.1 Quantitative Results

- **Source**: `wk09/analysis/findings.md` & `metrics.csv`

- Summary Table:

  | **Task**           | **HTMX (n=3)**   | **No-JS (n=1)** | **Insight**                                                  |
    | ------------------ | ---------------- | --------------- | ------------------------------------------------------------ |
  | **T1 (Filter)**    | ~3.7ms (Server)  | ~2.0ms (Server) | Server is fast in both; UX difference is client-side (instant update vs. reload). |
  | **T2 (Add/Error)** | 100% Success     | 100% Success    | Both groups recovered successfully. Error Summary (No-JS) proved effective. |
  | **T3 (Edit)**      | **100% Success** | **0% Success**  | **Critical Blocker**: No-JS user trapped in edit mode due to broken Cancel button. |
  | **T4 (Delete)**    | ~2.3ms           | **1.0ms**       | No-JS is dangerously fast because it **skips confirmation**, leading to user anxiety. |

- **Key Finding**: Task 3 (Edit) is technically broken for No-JS. Task 4 (Delete) is functionally successful but
  psychologically failing (Confidence 2/5).

### 2.2 Qualitative Analysis

- **Source**: `wk09/data/pilot-notes.md`
- **Theme 1: The "Confirmation Gap"**:
    - *Evidence*: P2 (No-JS) said "Wait, did I delete it? It didn't ask me..."
    - *Implication*: Immediate deletion via POST is terrifying for users. Needs a confirmation page.
- **Theme 2: The "Broken" Cancel Button**:
    - *Evidence*: P2 tried to click "Cancel" in Edit mode. Nothing happened.
    - *Implication*: Button relies on `hx-get` and lacks an `href` fallback. Critical functional bug.
- **Theme 3: Error Visibility (Positive)**:
    - *Evidence*: P2 noticed the "Big red box" (Error Summary) immediately.
    - *Implication*: The WCAG 3.3.1 implementation for No-JS is highly effective.

### 2.3 Accessibility Observations

- **Keyboard Navigation (P3)**:
    - ✅ "Skip to content" works.
    - ✅ Focus management in Error Summary is perfect (jumps to input).
    - ⚠️ Edit form tab order (Save vs Cancel) caused minor hesitation.
- **Screen Reader**:
    - ✅ Status messages injected into DOM are announced.
    - ❌ No-JS Cancel button is a "dead" control (focusable but inert).

------

## Section 3: Evidence Chains

### 3.1 High-Priority Findings

- **Source**: `wk09/analysis/evidence-chains.md`
- **Chain 1: No-JS Delete Safety**:
    - *Data*: P2 Confidence 2/5 + Quote "Did I delete it?".
    - *Backlog*: ID 14 (High Severity).
    - *Fix*: Implement `/tasks/{id}/delete/confirm` page.
    - *Screenshot*: `wk09/evidence/nojs-delete-surprise.png`.
- **Chain 2: No-JS Cancel Button**:
    - *Data*: P2 observation "Clicked cancel but nothing happened".
    - *Backlog*: ID 15 (High Severity).
    - *Fix*: Convert `<button>` to `<a href="..." role="button">` or use `formaction`.
    - *Screenshot*: `wk09/evidence/nojs-broken-cancel.png`.

### 3.2 Backlog Integration (10%)

- **Source**: `backlog/backlog.csv`
- **New Items Added**:
    - **ID 14**: Delete confirmation missing (Candidate Fix: Yes).
    - **ID 15**: Edit Cancel button broken (Candidate Fix: Yes).
    - **ID 16**: Filter submit button missing (Candidate Fix: No - Defer).
- **Prioritization**: Fixed critical functional bugs (Cancel) and safety issues (Delete) over minor affordance issues (
  Filter button).

------

## Section 4: Reflection

### 4.1 Process Critique

- **Successes**:
    - Server-side instrumentation (`metrics.csv`) worked flawlessly, capturing accurate server times independent of
      client network.
    - Protocol ensured ethical consent handling.
- **Limitations**:
    - Small sample size (n=4) means quantitative statistical significance is low.
    - "Peer" participants are tech-savvy; real users might struggle more with the No-JS filter.

### 4.2 Next Steps

- **Week 10 Redesign Plan**:
    1. **Fix #1 (Critical)**: Rewrite `_edit.peb` to use `formaction` for Cancel button to ensure No-JS support (Solves
       ID 15).
    2. **Fix #2 (Safety)**: Create a dedicated Pebble template for Delete Confirmation and route logic for No-JS (Solves
       ID 14).
    3. **Re-verify**: Run `nojs-check.md` script again to ensure fixes work.

------

## Files to Include

- `wk09/research/tasks.md`
- `wk09/research/measures.md`
- `wk09/research/protocol.md`
- `wk09/data/pilot-notes.md`
- `wk09/analysis/findings.md`
- `wk09/analysis/evidence-chains.md`
- Updated `backlog/backlog.csv`