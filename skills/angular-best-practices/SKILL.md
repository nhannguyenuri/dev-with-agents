---
name: angular-best-practices
description: Enforce modern Angular and TypeScript best practices, WCAG AA accessibility, Signal-based state and forms, and template optimization. Use when writing, refactoring, or reviewing Angular components, services, templates, forms, or TypeScript code.
license: MIT
compatibility: Compatible with Angular 20+.
metadata:
  author: Nhan Nguyen @ hoainhananguyen@gmail.com
  version: "1.0"
---

# Angular Best Practices

Build functional, maintainable, performant, and accessible web applications following modern Angular (v22+) and TypeScript standards.

## When to Use

- Generating new Angular components, directives, pipes, services, or routes
- Creating or refactoring forms using Signal Forms (`@angular/forms/signals`) or Reactive Forms
- Implementing local or derived state with Angular Signals
- Auditing or writing templates for WCAG AA and AXE accessibility compliance

## Core Standards

### 1. TypeScript Rules

- Enable and respect strict type checking across all files.
- Prefer type inference whenever the type is immediately obvious from the assignment.
- Never use `any`. Use `unknown` when a type is uncertain and narrow it with type guards before consumption.

### 2. Component Architecture

- Keep components small and focused on a single responsibility.
- Always author standalone components, directives, and pipes over `NgModule`.
- Omit `standalone: true` inside `@Component`, `@Directive`, and `@Pipe` decorators (default in Angular v20+).
- Omit `changeDetection: ChangeDetectionStrategy.OnPush` inside `@Component` decorators (default in Angular v22+).
- Define host bindings and listeners inside the `host` metadata object of `@Component` or `@Directive`. Never use `@HostBinding` or `@HostListener` decorators.
- Prefer inline templates (`template: ...`) for small components. When using external templates or stylesheets, always use relative paths (`./component.html`).
- Configure lazy loading (`loadComponent` / `loadChildren`) for all feature routes.

### 3. Component API & State Management

- Use `input()` and `output()` functions instead of `@Input()` and `@Output()` decorators.
- Use `model()` for two-way bound properties (`[(prop)]`) instead of pairing `input()` and `output()`.
- Use `signal()` for local mutable component state and `computed()` for pure, predictable derived state.
- Use `linkedSignal()` when state is derived from multiple reactive sources that must remain synchronized while still allowing manual updates.
- Update signals exclusively via `.set()` or `.update()`. Never call `.mutate()` on signals
