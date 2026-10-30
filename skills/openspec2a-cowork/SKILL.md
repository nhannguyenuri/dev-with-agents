---
name: openspec2a-cowork
description: End-to-end workflow integrating Azure DevOps ticket management, OpenSpec specification and implementation, and Chrome DevTools MCP testing. Use when processing Azure DevOps work items or tickets with OpenSpec and running automated E2E verification.
---

# OpenSpec Azure DevOps & E2E Workflow

Guideline for processing development tickets end-to-end by combining Azure DevOps MCP, OpenSpec implementation cycles, and Chrome DevTools MCP testing.

## Workflow Overview

Follow this strict 5-step sequence for every ticket assigned:

[Azure DevOps MCP] -> [OpenSpec Analysis & Impl] -> [Chrome DevTools E2E] -> [Checklist Handover] -> [Browser Teardown]

---

## Execution Steps

### Step 1: Fetch Ticket via Azure DevOps MCP

1. Use Azure DevOps MCP tools (`get_work_item`, `get_work_item_details`, or equivalent) with the provided Work Item ID.
2. Extract essential parameters:
   - **Title & Description**: Core functional intent.
   - **Acceptance Criteria (AC)**: Explicit testable assertions.
   - **Related Links / PRs / Branches**: Base branch or target environment.
3. Validate that requirements are clear enough before coding; ask for clarification if AC is missing or ambiguous.

### Step 2: OpenSpec Process & Implementation

1. Initialize or update OpenSpec artifacts (`proposal.md`, `specs/`, or task lists) matching project conventions.
2. Formulate explicit changes:
   - Identify affected modules, interfaces, and test requirements.
   - Document the implementation plan according to OpenSpec guidelines.
3. Execute the code changes in the repository.
4. Verify standard unit tests and linter checks pass before proceeding to E2E verification.

### Step 3: E2E Testing with Chrome DevTools MCP

1. Ensure the target web application is running locally or deployed to the test target URL.
2. Launch and connect via Chrome DevTools MCP:
   - Navigate to the page/flow modified by the ticket.
   - Simulate user interactions (click, type, navigate, form submission) matching each Acceptance Criteria.
   - Inspect console logs (`console.error`, uncaught exceptions) and network traffic (API response status codes, payload structures).
3. Capture visual state or DOM snapshots if needed to verify UI rendering and layout.
4. If tests fail or regressions occur, iterate on Step 2 and re-test.

### Step 4: Verification Summary & Developer Checklist

Prepare a clean Markdown handover report for the developer or reviewer:

```markdown
### Ticket Verification Summary

- **Work Item**: #[ID] - [Title]
- **Status**: Ready for Code Review & QA

#### OpenSpec Artifacts

- [x] Spec updated/created: `specs/...`
- [x] Code implementation matches acceptance criteria

#### E2E Verification (Chrome DevTools MCP)

- [x] Scenario 1: [Short description of AC 1 verified]
- [x] Scenario 2: [Short description of AC 2 verified]
- [x] Console check: 0 errors detected
- [x] Network check: API payloads and responses validated

#### Developer Review Checklist

- [ ] Code style and naming standards verified
- [ ] Edge-cases or fallback states manually sanity-checked
- [ ] Ready to link/associate with Azure DevOps PR
```
