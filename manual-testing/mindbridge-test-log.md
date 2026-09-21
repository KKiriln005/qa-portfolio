# Test Log: MindBridge

| | |
|---|---|
| **Tester** | KKiriln005 |
| **Date** | 21 Sep 2026 |
| **Application under test** | MindBridge (course catalog, local build at `http://localhost:3000`) |
| **Feature under test** | Course search and page layout |
| **Test approach** | Manual testing, browser DevTools (Network tab) for timing and responsive checks |

## Summary

| ID | Test | Type | Result |
|----|------|------|--------|
| TC-01 | Search returns matching courses | Functional | Pass |
| TC-02 | Search response time | Non-functional (performance) | Fail |
| TC-03 | Responsive layout at narrow width | Non-functional (usability) | Fail |

**Overall:** 1 passed, 2 failed. The search feature works correctly, but it is slow and the layout breaks on small screens.

## Defects found

| Defect | Related test | Area |
|--------|--------------|------|
| Search request takes about 3 seconds | TC-02 | Performance |
| Course cards are cut off below 768 px and the page scrolls sideways | TC-03 | Responsive layout |

---

## TC-01: Search returns matching courses

| Field | Details |
|-------|---------|
| **Type** | Functional |
| **Search term** | Software Testing |
| **Expected result** | The list refreshes and shows courses whose title or description matches the keyword. Each card shows a course title, subject, level, and price. |
| **Actual result** | The search returned matching courses. |
| **Result** | **Pass** |

## TC-02: Search response time

| Field | Details |
|-------|---------|
| **Type** | Non-functional (performance) |
| **Search term** | Software Testing |
| **Expected result** | The `/api/courses` request completes in under 1 second. |
| **Actual result** | The search completed, but the `courses` request (status 200, fetch, 7.3 kB) took **3030 ms**. Timing tab: waiting for server response 3.02 s, content download 1.50 ms. |
| **Result** | **Fail** |
| **Observation** | Almost all of the time is server wait, not download. A 3-second delay is noticeable to users and too slow for a real product. |

## TC-03: Responsive layout at narrow width

| Field | Details |
|-------|---------|
| **Type** | Non-functional (usability / responsive) |
| **Widths tested** | 375 px (below the 768 px breakpoint), and 768 px and wider |
| **Expected result** | Course cards stack cleanly, all text stays inside its container, and there is no horizontal scrollbar. |
| **Actual result** | At 375 px the layout breaks. The course card is cut off on the left: the title ("...ral Software Testing platforms"), the subject tag ("...oftware Testing") and the description text run outside the card, and a horizontal scrollbar appears. At 768 px and wider the layout looks correct. |
| **Result** | **Fail** |
| **Observation** | The problem only appears below 768 px. The user has to scroll sideways to read the content. |
