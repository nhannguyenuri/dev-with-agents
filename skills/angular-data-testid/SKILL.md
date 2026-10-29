---
name: angular-data-testid
description: Enforces rules, naming conventions, and Angular template best practices for adding stable data-testid attributes to HTML elements and components for automated E2E and integration testing (Playwright, Cypress, Testing Library).
---

# Skill: Adding `data-testid` Attributes for Test Automation (Angular)

## 1. Core Guardrails
- **Zero Behavioral Impact:** ONLY add or update `data-testid` attributes. Never modify component logic, Signals, RxJS streams, inputs/outputs, event handlers, CSS classes, or DOM hierarchy.
- **No CSS or TS Coupling:** Never reference `data-testid` inside component stylesheets (`.css`, `.scss`, Tailwind) or TypeScript business logic. It exists strictly for test runners.
- **Preserve Existing Test IDs:** Do not rename or remove existing `data-testid` attributes unless explicitly instructed, as doing so will break existing QA test suites.
- **Standardized Attribute:** Always use `data-testid` (never mix `data-test`, `data-qa`, or `data-cy` unless legacy files explicitly require it).

## 2. Naming Convention (`kebab-case`)
All `data-testid` values must use lowercase `kebab-case` and follow a predictable hierarchical pattern:

**Pattern:** `{context/feature}-{component/element}-{action/type}`

- **Semantic over Visual:** Name elements after their domain purpose, not their visual style or position (e.g., use `checkout-submit-btn` instead of `green-right-btn`).
- **Decouple from Localization:** Never bind `data-testid` to `ngx-translate` pipes, `$localize`, or mutable UI text.

### Standard Suffixes by Element Type
| Element Type | Suffix / Pattern | Example |
| :--- | :--- | :--- |
| **Buttons / Actions** | `...-btn` or `...-button` | `data-testid="login-submit-btn"` |
| **Text / Select Inputs** | `...-input` or `...-select` | `data-testid="profile-email-input"` |
| **Checkboxes / Radios** | `...-checkbox` / `...-radio` | `data-testid="terms-accept-checkbox"` |
| **Links / RouterLinks** | `nav-...-link` | `data-testid="nav-settings-link"` |
| **Containers / Sections** | `...-container` or `...-section` | `data-testid="cart-summary-section"` |
| **Modals / Dialogs** | `...-modal` or `...-dialog` | `data-testid="delete-confirm-modal"` |
| **Alerts / Banners / Errors** | `...-error` or `...-alert` | `data-testid="payment-failed-alert"` |
| **Lists & Tables** | `...-list` / `...-table` | `data-testid="invoices-table"` |
| **Dynamic List Items / Rows** | `...-item-{id}` / `...-row-{id}` | `[attr.data-testid]="'invoice-row-' + invoice.id"` |
| **Loading / Empty States** | `...-loading` / `...-empty` | `data-testid="search-results-empty"` |

## 3. Target Selection: What Gets a `data-testid`?
Do **not** pollute the DOM by adding `data-testid` to every `<div>` or `<span>`. Apply them selectively to:
1. **Interactive Controls:** `<button>`, `<input>`, `<select>`, `<textarea>`, `<a routerLink="...">`, and custom form controls (`formControlName`, `ngModel`).
2. **Dynamic Data Displays:** Elements whose interpolated text (`{{ value() }}`) or state is asserted in tests (e.g., total price counters, status badges, `<mat-error>` validation messages, toast notifications).
3. **Scope Containers:** Major landmark wrappers (modals, `<form>`, cards, table bodies) that help QA scope locators (e.g., `page.getByTestId('user-modal').getByTestId('save-btn')`).
4. **Conditional UI (`@if` / `@switch`):** Loading spinners, skeleton screens, and `@empty` state placeholders.

*Skip purely decorative elements (`<mat-icon>` inside buttons, layout wrappers, dividers, static paragraphs).*

## 4. Angular-Specific Template & Binding Rules

### Static vs. Dynamic Binding
- **Static Strings:** Always use plain HTML attributes without brackets for static IDs to avoid unnecessary Angular change-detection overhead.
  - **Good:** `data-testid="user-save-btn"`
  - **Bad:** `[attr.data-testid]="'user-save-btn'"` or `[data-testid]="'user-save-btn'"`
- **Dynamic Expressions:** Because `data-testid` is an HTML attribute and not a DOM property, you **must** use `[attr.data-testid]` (never `[data-testid]`, which will throw an Angular template binding error).
  - **Good:** `[attr.data-testid]="'invoice-row-' + invoice.id"`

### Modern Control Flow (`@for`, `@if`, `@switch`)
- **Inside `@for` Loops:** Prefer binding a unique domain identifier (`item.id`, `item.sku`, `item.code`) rather than `$index`, as `$index` shifts when lists are sorted, filtered, or paginated.
  ```html
  @for (user of users(); track user.id) {
    <tr [attr.data-testid]="'user-row-' + user.id">
      <td [attr.data-testid]="'user-name-' + user.id">{{ user.name }}</td>
      <td>
        <button type="button" [attr.data-testid]="'user-Delete-btn-' + user.id" (click)="deleteUser(user.id)">
          Delete
        </button>
      </td>
    </tr>
  } @empty {
    <div data-testid="users-table-empty">No users found.</div>
  }
