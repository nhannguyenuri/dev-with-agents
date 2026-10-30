---
name: p2a-cowork
description: Execute end-to-end development workflows for tickets from Azure DevOps. Handles research, technical planning, user approval, implementation, unit testing, DevTools testing, and delivery tracking.
---

# DevOps Ticket Implementer

An end-to-end workflow skill for autonomous software engineering. Guides the agent through ticket retrieval via Azure DevOps MCP, domain & tech research, architecture planning, human confirmation, coding, multi-layer verification (Unit + Chrome DevTools MCP), and final handoff checklist.

## When to Use

- Resolving user stories, bugs, or feature tickets fetched from Azure DevOps.
- The user provides an Azure DevOps ticket ID or asks to work on an assigned issue.
- Structured execution requiring dev approval before writing code and automated browser validation before closing.

---

## Workflow Steps

### Step 1: Ticket Ingestion & Context Loading
- Call the installed Azure DevOps MCP tool to fetch the ticket details by ID (title, description, acceptance criteria, comments, parent epics/features).
- Extract explicit constraints, scope boundaries, and acceptance criteria.
- Summarize the ticket context back to the developer briefly to confirm ingestion.

### Step 2: Research & Domain Exploration
- Analyze required domain logic, existing rules, and installed skills in the codebase.
- Identify relevant libraries, third-party packages, or modern framework patterns suitable for the task.
- Inspect the codebase to identify existing conventions, utilities, and potential points of conflict.

### Step 3: Plan Specification
- Draft a detailed **Plan Spec** outlining:
  - Task breakdown (milestones and atomic sub-tasks).
  - Pre-requisites and dependencies.
  - Potential edge cases and mitigation strategies.
  - Acceptance criteria validation map.

### Step 4: Architecture & Module Planning
- Define the technical design:
  - File/module hierarchy to create or modify.
  - Data contracts (interfaces, schemas, database models, DTOs).
  - State management, API endpoints, or event flows.
  - Integration touchpoints with existing services.

### Step 5: Human-in-the-Loop Confirmation
- Present the combined Plan & Architecture (Steps 3 & 4) clearly to the developer.
- **HALT EXECUTION:** Do not proceed to code implementation until the developer explicitly approves the plan or provides adjustments.

### Step 6: Implementation
- Implement the approved changes incrementally according to the codebase style.
- Write clean, type-safe, and self-documenting code.
- Avoid modifying unrelated files; keep git diff scoped to the ticket requirements.

### Step 7: Unit Testing
- Execute existing unit tests to prevent regressions.
- Author new unit/integration tests covering:
  - Happy paths matching ticket criteria.
  - Failure/edge scenarios identified during Step 3.
- Run tests in the terminal and verify all test suites pass with 0 errors.

### Step 8: E2E / Browser Testing via Chrome DevTools MCP
- Launch and inspect the application using the Chrome DevTools MCP tool.
- If authentication is required, prompt the developer: *"Please provide test credentials or log in to the test session."*
- Inspect DOM elements, network requests, and console error logs.
- Capture screenshots or DOM logs verifying that acceptance criteria are met in the UI.

### Step 9: Post-Execution Tracking Checklist
- Provide a clear, actionable checklist for the developer covering:
  - [ ] Acceptance criteria verification status.
  - [ ] Test coverage & unit test results.
  - [ ] Chrome DevTools verification artifacts (console logs, UI states).
  - [ ] Code review checklist & potential performance/security optimizations.
  - [ ] Suggested git commit message / PR description.

### Step 10: Clean Session Teardown
- Ensure all test processes, local mock servers, and Chrome DevTools instances/browser windows opened during Step 8 are cleanly closed.
- Provide a final wrap-up statement summarizing the completed ticket.

---

## Gotchas & Rules

- **Do Not Skip Step 5:** Never write code before the human developer gives explicit approval on the plan.
- **Credential Safety:** Never hardcode or echo sensitive credentials in logs or git commits during Step 8.
- **Fail Early:** If Azure DevOps MCP or Chrome DevTools MCP fails to respond, alert the developer immediately with the specific error rather than assuming mock data.
