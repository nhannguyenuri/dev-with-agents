---
name: angular-architecture
description: Enforce modern Angular standards (Signals, Standalone Components, inject(), modern control flow syntax) and strict directory boundaries and unidirectional dependency rules under src/app. Use when generating, refactoring, reviewing, or organizing Angular components, services, stores, HTTP clients, utilities, or routing.
license: MIT
compatibility: Compatible with Angular 20+.
metadata:
  author: Nhan Nguyen @ hoainhananguyen@gmail.com
  version: "1.0"
---

# Angular Architecture

Follow modern Angular standards and enforce strict directory boundaries and unidirectional dependency flow across `src/app`.

## When to Use

- Generating new Angular components, views, services, state stores, HTTP clients, or utilities
- Refactoring existing Angular code to modern patterns (Signals, Standalone, `inject()`, `@if`/`@for`)
- Reviewing Angular code or import paths for architectural boundary violations
- Designing feature modules, state management, or API integration layers in an Angular repository

## Modern Angular Standards

Apply these core coding patterns across all generated or refactored Angular code:

- **Standalone by Default**: Declare all components, directives, and pipes as standalone. Do not introduce `NgModule`.
- **Dependency Injection**: Use the `inject()` function instead of constructor parameter injection.
- **Reactivity with Signals**: Prefer Angular Signals (`signal`, `computed`, `effect`, `input`, `output`, `model`) over RxJS `BehaviorSubject` or `@Input()` / `@Output()` decorators for synchronous state and component communication.
- **Modern Control Flow**: Use built-in template control flow syntax (`@if`, `@else`, `@for`, `@switch`, `@defer`) instead of structural directives (`*ngIf`, `*ngFor`, `*ngSwitch`).
- **Change Detection**: Set `ChangeDetectionStrategy.OnPush` on all components to ensure optimal rendering performance with Signals.

## Directory Structure & Boundaries

Place every file under `src/app` in its designated architectural layer. Respect both permitted contents and forbidden dependencies:

| Path                    | Purpose & Permitted Contents                                                                                                   | Forbidden Dependencies                                        |
| :---------------------- | :----------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------ |
| `src/app/core/http/`    | API clients, HTTP interceptors, endpoint constants, OpenAPI contracts, and network models.                                     | Feature stores, view components, UI SDKs.                     |
| `src/app/features/`     | Domain-level feature state management (Signal stores, NgRx, Akita), feature-specific constants, validators, and domain models. | Direct UI page rendering or route configs.                    |
| `src/app/utils/`        | Framework-agnostic helpers, formatting pipes, shared regex patterns, date/math utilities.                                      | UI components, HTTP services, feature stores.                 |
| `src/app/integrations/` | Third-party SDK wrappers (e.g., Firebase, Stripe, Analytics, Auth0, Sentry).                                                   | Business logic, page layouts.                                 |
| `src/app/views/`        | Route-level page components, page containers, and routing definitions.                                                         | Reusable dumb UI components (delegate down to `components/`). |
| `src/app/components/`   | Presentational / reusable dumb components (buttons, modals, form fields, cards).                                               | Route-level logic, direct API calls, feature stores.          |

## Unidirectional Dependency Flow

Enforce a strict top-down dependency graph so lower layers never import from higher layers:

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
```
