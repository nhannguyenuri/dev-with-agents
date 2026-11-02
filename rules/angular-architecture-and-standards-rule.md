---
trigger: always_on
description: Enforce modern Angular standards (Signals, Standalone Components, inject(), modern control flow syntax) and strict directory boundaries with unidirectional dependency rules under src/app.
license: MIT
compatibility: Compatible with Angular 20+.
metadata:
  author: Nhan Nguyen @ hoainhananguyen@gmail.com
  version: "1.0"
---

# Angular Architecture & Standards Rule

You must strictly adhere to modern Angular standards and enforce architectural directory boundaries and unidirectional dependency flow across `src/app` whenever generating, refactoring, or reviewing Angular code.

## 1. Modern Angular Coding Standards

- **Standalone by Default**: Declare all components, directives, and pipes as standalone (`standalone: true` or implicit in Angular v19+). Never create or introduce `NgModule`.
- **Dependency Injection**: Always use the `inject()` function instead of constructor parameter injection.
- **Reactivity with Signals**: Use Angular Signals (`signal`, `computed`, `effect`, `input`, `output`, `model`) over RxJS `BehaviorSubject` or `@Input()` / `@Output()` decorators for state management and component communication.
- **Modern Control Flow**: Exclusively use built-in template control flow syntax (`@if`, `@else`, `@for`, `@switch`, `@defer`). Do not use legacy structural directives (`*ngIf`, `*ngFor`, `*ngSwitch`).
- **Change Detection**: Always configure `changeDetection: ChangeDetectionStrategy.OnPush` on every component decorator.

## 2. Directory Structure & Architectural Boundaries

Place every file under `src/app` in its designated architectural layer and strictly respect dependency restrictions:

| Layer Path | Permitted Contents | Forbidden Imports / Dependencies |
| :--- | :--- | :--- |
| `src/app/views/` | Route-level page components, page containers, and routing definitions. | Defining reusable dumb UI components inline (must delegate to `components/`). |
| `src/app/components/` | Presentational / reusable dumb UI components (buttons, modals, form fields, cards). | `views/`, `features/` stores, `core/http/` services, route-level logic. |
| `src/app/features/` | Domain-level state management (Signal stores, NgRx, Akita), domain models, feature validators, and constants. | `views/`, `components/`, route configs, direct UI rendering. |
| `src/app/core/http/` | API clients, HTTP interceptors, endpoint constants, OpenAPI contracts, and network DTOs. | `views/`, `components/`, `features/` stores, UI SDKs. |
| `src/app/integrations/` | Third-party SDK wrappers (e.g., Firebase, Stripe, Analytics, Auth0, Sentry). | `views/`, `components/`, `features/`, business logic, page layouts. |
| `src/app/utils/` | Pure, framework-agnostic helpers, formatting pipes, shared regex patterns, date/math utilities. | `views/`, `components/`, `features/`, `core/http/`, `integrations/`. |

## 3. Unidirectional Dependency Flow

Enforce a strict top-down dependency graph. Lower layers must never import from higher layers:

```text
[views] ──────────────┐
   │                  ▼
   │            [components]
   │                  │
   ▼                  ▼
[features] ───► [core/http] ───► [integrations]
   │
   ▼
[utils]
