# QA Strategy and Execution — MindBridge Shareable Content Feature

## About

A QA strategy and execution document for a fictional feature that lets
students generate a shareable link to their dashboard content.

This artifact was produced as a lab + graded assignment for the
[Microsoft Junior QA/Software Tester Professional Certificate](https://www.coursera.org/professional-certificates/microsoft-junior-qa-software-tester)
on Coursera.

## What it demonstrates

- **RACI matrix** — assigning Responsible / Accountable / Consulted /
  Informed roles across UI, API, and DB layers for QA vs. Dev.
- **STLC strategy** — naming static techniques (requirement review, peer
  code review), dynamic techniques (functional, integration testing),
  and the test environment, each with a stated reason.
- **BDD acceptance criteria** — three Given-When-Then scenarios (happy
  path, edge case, error condition) written into a live Jira ticket.
- **Defect discovery and evidence** — two real defects found while
  executing the BDD scenarios against the staging build, each
  documented with repro steps, expected vs. actual result, severity
  reasoning, and a screenshot:
  - `[API]` an unhandled HTTP 500 triggered by an empty share-note field
  - `[UI]` a share-link URL that overflows its container below 400px
    viewport width

## Files

| File | Purpose |
|---|---|
| [`qa-strategy-and-execution.md`](./qa-strategy-and-execution.md) | Full document, readable directly on GitHub |
| `QA_Strategy_and_Execution_Document.pdf` | Submitted version, as graded |

## Skills

`RACI` · `STLC Planning` · `BDD / Gherkin` · `Defect Documentation` ·
`Jira` · `Responsive & API Defect Triage`

