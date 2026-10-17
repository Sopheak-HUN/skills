Act as a Principal Frontend Engineer specializing in Nuxt 4 (`4.6.1`), Vue 3 (`<script setup lang="ts">`), Tailwind CSS (`darkMode: 'class'`), Pinia, and `@nuxtjs/i18n`.

First, read the complete specification in `~/Project/own/skills/pos-admin-dashboard/SKILL.md` and treat it as your strict architectural contract.

### 1. Project Detection & Boundaries
- Inspect the current working directory first:
  - **If an existing Nuxt project is present:** Inspect `package.json`, `nuxt.config.ts`, `tailwind.config.ts`, and `app/`. Preserve working code, `<slot />` outlets, and existing conventions.
  - **If empty / no Nuxt project exists:** Initialize a frontend-only Nuxt 4 project in the current directory (`npm create nuxt@latest .` or equivalent) and install `@pinia/nuxt`, `pinia`, `@nuxtjs/i18n`, and Tailwind CSS.
- **Strict Boundaries:** Frontend only. Do NOT scaffold Laravel, databases, Docker, or CI/CD. Do NOT install UI component libraries (the single exception is `vue-multiselect@^3.5.0`, used for every select) or add languages beyond `en` and `km` or themes beyond `light` and `dark`.

### 2. Core Styling & Zero-Bug Layout Rules
- **Centralized `@apply` in `app/assets/css/main.css`:** Define CSS custom properties (`:root` and `.dark`) and all layout, component, table, modal, permission matrix, and input classes in `main.css` using `@apply` with `dark:` companions. Do not scatter long utility strings across Vue templates, and never `@apply` one custom class inside another custom class.
- **iOS/iPadOS Safari Auto-Zoom Prevention:** Every `<input:not([type="checkbox"]):not([type="radio"])>` and `<textarea>` (`.apple-input`, `.apple-search-input`, and the `vue-multiselect` `.multiselect__input`) MUST enforce `text-base lg:text-sm` (`16px` below `1024px`) and `min-h-[44px]` so mobile/tablet Safari never auto-zooms on focus.
- **Selects via `vue-multiselect`:** Never render a native `<select>`. Every picker uses `app/components/shared/AppSelect.vue`, a wrapper around `vue-multiselect` `^3.5.0` that binds primitive keys through `v-model`, passes a unique `useId()`, sets `:show-labels="false"`, and translates the placeholder and `noResult` / `noOptions` / `maxElements` slots. Do not import the library's CSS. Style `.multiselect*` in `main.css` outside `@layer` so Tailwind never purges the runtime-only classes.
- **Rounded Checkboxes:** `.apple-checkbox` is a native `<input type="checkbox">` with `appearance-none`, `rounded-md`, a blue fill and white SVG check when `:checked`, a dash when `:indeterminate`, and `dark:` companions. Wrap it in `.checkbox-field` for a 44px touch target.
- **Floating Left Sidebar & Exact Viewport Offsets (`app/layouts/default.vue`):**
  - Expanded desktop sidebar: `fixed top-3 bottom-3 left-3 z-40 w-64` (`16.75rem` footprint) -> `.main-viewport` uses `lg:pl-72` (`18rem`).
  - Collapsed desktop icon rail: `lg:w-20` (`5.75rem` footprint) -> `.main-viewport-collapsed` uses `lg:pl-28` (`7rem`).
  - Mobile off-canvas state: `-translate-x-[calc(100%+1.5rem)]` over `.sidebar-backdrop` (`z-30`) so the `left-3` offset and shadow never bleed onto mobile screens.
  - Overflow & SSR safety: Always apply `min-w-0 flex-1 overflow-x-hidden` on `.main-viewport` and `.page-content`. Initialize layout state with `useState` (never read `window.innerWidth` during SSR).
- **Full-Screen POS Mode (`/pos` & `/pos/**`):**
  - Hide `AppSidebar.vue` and `.sidebar-backdrop` completely; apply `.main-viewport-pos` (`lg:pl-6`).
  - In `AppTopbar.vue`, render `.pos-gear-btn` (`aria-haspopup="menu"`, `aria-expanded`) that opens `.pos-gear-dropdown` (`z-50`) with an **Exit** action (`.pos-gear-dropdown-exit`) redirecting to `/` via `await navigateTo('/')`. Close on outside click, `Escape`, or route change.

### 3. JSONC Menu, RBAC, i18n & Theme System
- **JSONC Menu (`app/config/menu.jsonc` & `app/utils/jsonc.ts`):** Implement `parseJsonc<T>()` to safely strip `//` and `/* */` comments without corrupting string literals. Filter sidebar groups/items in `useMenu.ts` via `usePermissions.ts` (`permission.view` or `"*"`).
- **i18n (`@nuxtjs/i18n`) & Dual Theme (`app/stores/preferences.ts`):**
  - Configure `en` (`app/locales/en.json`) and `km` (`app/locales/km.json`) with `strategy: 'no_prefix'`. Use `$t()` / `t()` for all visible text—zero hardcoded strings.
  - Persist `pos-locale` and `pos-theme` via SSR-safe `useCookie` + client `localStorage` sync in `preferences.ts`.
  - Wire `useLocale.ts`, `useTheme.ts` (`useHead` syncing `htmlAttrs.class` with `'dark'`), `LangSwitcher.vue`, and `ThemeToggle.vue` in `app.vue` and `AppTopbar.vue`.

### 4. MVC-Inspired Modules & Users/Roles CRUD
- Structure code cleanly into `app/types/`, `app/utils/`, `app/services/` (with in-memory `app/services/demo/*` simulating latency and `422` Laravel-compatible validation errors), `app/stores/`, `app/composables/`, and `app/pages/`.
- Implement the Role Switcher in the sidebar footer (`admin` with `"*"`, `manager`, `cashier`, `inventory_clerk`) to test live permission reactivity.
- Build full CRUD for `/users` (`index`, `new`, `[id]`) and `/roles` (`index`, `new`, `[id]`) with `PermissionMatrix.vue` dynamically generated from `menu.jsonc` (`view`, `create`, `edit`, `delete` + row/column "Select all" with indeterminate state), using rounded `.apple-checkbox` cells.
- Enforce all 4 destructive safety guards in the UI and demo services:
  1. Block deleting the currently signed-in user.
  2. Block deleting/deactivating the last active user holding `users.edit`.
  3. Block renaming or deleting a system role (`isSystem: true`).
  4. Block deleting a role with assigned users (`usersCount > 0`).
- Build the `/` Overview dashboard, `/pos` terminal screen, and operational modules (`/products`, `/categories`, `/sales`, `/orders`, `/inventory`, `/customers`, `/reports`, `/settings`).

### 5. Execution & Verification
Execute this in the 7 phases defined in `SKILL.md`. After completing the implementation, run `npm run typecheck`, `npm run lint` (if configured), and `npm run build`, fix any errors, and output a final verification summary covering responsive offsets, Safari `>= 16px` input compliance, zero native `<select>` elements with all pickers on `AppSelect.vue`, rounded checkbox styling, `en`/`km` translation completeness, Light/Dark mode parity, and RBAC safety guards.