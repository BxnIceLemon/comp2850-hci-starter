# Qualitative Themes — Week 10

## Theme 1: Delete confirmation (No-JS)

Frequency: 1/1 No-JS participant (100% of relevant group)

Severity: High (Safety & Trust)

Evidence:

- P2 (No-JS): "Wait, did I delete it? It didn't ask me 'are you sure?'"

- Observation: User checked the list twice to confirm deletion, indicating anxiety.

  Problem: The immediate POST deletion in No-JS mode violates the user's mental model of safety established by modern
  apps. It creates a "scary" experience.

  Recommendation: Implement a dedicated Delete Confirmation Page (GET /tasks/{id}/delete/confirm) for No-JS users to
  restore parity in safety.

## Theme 2: "Cancel" Functionality Broken (No-JS)

Frequency: 1/1 No-JS participant

Severity: Critical (Functional Blocker)

Evidence:

- P2 (No-JS): "I clicked cancel but it didn't do anything... is it broken?"

- Observation: User was trapped in the edit view until they navigated away or submitted.

  Problem: The "Cancel" button is implemented as a \<button hx-get="..."\>, which relies entirely on JavaScript. Without
  JS, it is an inert element.

  Recommendation: Convert the button to a semantic link (\<a href="/tasks"\>) that progressively enhances to HTMX when
  JS is available.

