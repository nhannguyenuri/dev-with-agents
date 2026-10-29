---
trigger: always_on
description: "Modern Angular (v20+) file and symbol naming conventions."
---

# Angular 20+ Naming Conventions

Always strictly adhere to the modern Angular Style Guide (v20+) when generating, refactoring, or naming files and symbols.

## 1. General Rules
1. **File Case:** Use `kebab-case` for all file and directory names. Never use `camelCase`, `PascalCase`, or `snake_case` in filenames.
2. **No Type Suffixes for Core Building Blocks:** Do NOT append `.component`, `.service`, or `.directive` to filenames, and do NOT append `Component`, `Service`, or `Directive` to class names.
3. **Matching Prefixes:** Group related component files (TypeScript, template, styles, unit tests) in the same directory using the exact same `kebab-case` base name.
4. **Intent-Driven Naming:** Name services and utilities after their specific domain or responsibility (e.g., `user-api.ts`, `cart-state.ts`, `auth.ts`) rather than generic terms.

## 2. File & Symbol Mapping
Follow this exact pattern when creating files and exporting classes or functions:

- **Component:**
  - Logic: `<name>.ts` -> `export class <Name>` (e.g., `user-profile.ts` -> `export class UserProfile`)
  - Template: `<name>.html` (e.g., `user-profile.html`)
  - Styles: `<name>.scss` or `<name>.css` (e.g., `user-profile.scss`)
- **Service:**
  - File: `<name>.ts` -> `export class <Name>` (e.g., `user-api.ts` -> `export class UserApi`)
- **Directive:**
  - File: `<name>.ts` -> `export class <Name>` (e.g., `click-outside.ts` -> `export class ClickOutside`)
- **Pipe:**
  - File: `<name>-pipe.ts` -> `export class <Name>Pipe` (e.g., `currency-format-pipe.ts` -> `export class CurrencyFormatPipe`)
- **Guard (Functional):**
  - File: `<name>-guard.ts` -> `export const <name>Guard: CanActivateFn` (e.g., `auth-guard.ts` -> `export const authGuard`)
- **Interceptor (Functional):**
  - File: `<name>-interceptor.ts` -> `export const <name>Interceptor: HttpInterceptorFn` (e.g., `auth-interceptor.ts` -> `export const authInterceptor`)
- **Resolver (Functional):**
  - File: `<name>-resolver.ts` -> `export const <name>Resolver: ResolveFn<T>` (e.g., `user-resolver.ts` -> `export const userResolver`)
- **Routes:**
  - File: `<feature>.routes.ts` or `app.routes.ts` -> `export const routes: Routes`
- **Models / Types / Interfaces:**
  - File: `<name>.ts` or `<name>.model.ts` -> `export interface <Name>` / `export type <Name>` (Never prefix interfaces with `I`).
- **Unit Tests:**
  - File: `<exact-file-prefix>.spec.ts` (e.g., `user-profile.spec.ts`, `auth-guard.spec.ts`, `currency-format-pipe.spec.ts`)
