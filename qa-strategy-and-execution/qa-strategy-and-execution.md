# QA Strategy and Execution Document

**Project:** MindBridge — Shareable Content Feature
**Prepared By:** Kirill Kirilin
**Date:** September 23, 2026
**Jira Project:** ProjectK (Jira key: PROJ)

---

## Section 1: Architecture and RACI Matrix

| Layer | QA Team Role | Dev Team Role |
|---|---|---|
| UI | R | A |
| API | C | R |
| DB | I | A |

---

## Section 2: STLC Strategy and Environments

**Static testing technique(s):**
- Requirement and BDD-scenario review — walk through the Given-When-Then acceptance criteria with Dev and Product before coding starts, to catch ambiguity early
- Peer code review — Dev reviews the share-link generation and rate-limiting logic before merge; QA checks the review against the acceptance criteria

**Dynamic testing technique(s):**
- Functional (black-box) testing — manually execute all three BDD scenarios (happy path, edge case, error condition) against the built feature
- Integration testing — verify the UI "Generate share link" button correctly calls the API and renders the returned link or error message

**Test environment:**
- Staging — a pre-production environment that mirrors production configuration and uses seeded test data, so BDD scenarios can be executed safely before release

---

## Section 3: BDD Acceptance Criteria

Ticket **PROJ-2** was updated with three Given-When-Then scenarios:

**Scenario 1: Happy path — learner generates a share link successfully**
```gherkin
Given a logged-in learner is viewing their dashboard with completed activity
When they select "Generate share link"
Then the app displays a shareable link that the learner can copy and send to others
```

**Scenario 2: Edge case — learner reaches the active share link limit**
```gherkin
Given a learner has already generated the maximum number of active share links allowed (5)
When they select "Generate share link" again
Then the app displays a message that the link limit has been reached and does not create an additional link
```

**Scenario 3: Error condition — dashboard data fails to load**
```gherkin
Given a learner's dashboard data fails to load because of a server error
When they select "Generate share link"
Then the app displays an error message and does not generate a share link until the dashboard loads successfully
```

*Screenshot: Jira ticket PROJ-2 showing all three scenarios in the description field — see PDF version.*

---

## Section 4: Defect Log and Execution Evidence

### Defect 1 — `[API]` Empty share note triggers HTTP 500 (unhandled server error)

| Field | Detail |
|---|---|
| **Description** | Selecting "Generate share link" with an empty note field does not show a validation message. Instead, the request to the share-link API fails with a server-side error, visible in the Network tab. |
| **Steps to reproduce** | 1. Open the Dashboard and clear the share note field so it is empty.<br>2. Open DevTools → Network tab.<br>3. Select "Generate share link." |
| **Expected result** | The app validates the empty note (client-side, or the API returns a 4xx response) and shows a message asking the learner to enter a note. No server error should occur. |
| **Actual result** | The fetch request to `/api/dashboard/share-link` returns HTTP 500. No error message is shown to the learner — the request simply fails silently in the UI. |
| **Severity** | High — unhandled server-side exception on a core action, not just a missing validation message. |

*Screenshot: Network tab showing the share-link fetch request returning HTTP 500 — see PDF version.*

### Defect 2 — `[UI]` Share link text overflows container at narrow viewport widths

| Field | Detail |
|---|---|
| **Description** | On the Dashboard, after selecting "Generate share link," the confirmation banner displays the full share URL on a single line. At a 400px viewport width, the URL is cut off by the right edge of the banner instead of wrapping or truncating gracefully. |
| **Steps to reproduce** | 1. Open the Dashboard at `localhost:3000/dashboard`.<br>2. Set the browser viewport to 400px width (DevTools device toolbar).<br>3. Select "Generate share link." |
| **Expected result** | The share link text wraps to a new line, or is truncated with an ellipsis and a "Copy" button, so the full banner remains readable within the container. |
| **Actual result** | The link text overflows the banner and is cut off by the container edge; part of the URL is not visible or accessible to the user. |
| **Severity** | Medium — the feature still functions (the link is generated), but the user cannot read or reliably copy the full URL at this viewport width, which is a usability/responsive-design issue rather than a functional failure. |

*Screenshot: Dashboard at 400px viewport width showing the share link URL cut off by the banner's right edge — see PDF version.*
