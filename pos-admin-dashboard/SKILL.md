---
name: pos-admin-dashboard
description: Design, initialize, build, and iteratively improve a POS admin dashboard using Nuxt 4, Tailwind CSS (@apply in main.css), Pinia, a floating left sidebar driven by a JSONC menu config with module names and view permissions, a users & roles CRUD module with role-based action permissions (view/create/edit/delete), full-screen POS mode without sidebar and with a gear icon dropdown to exit to admin, Apple-style inputs that prevent iOS Safari auto-zoom on focus, and bug-free responsive layouts across mobile, tablet, and PC. Use for POS dashboard layouts, products, sales, orders, inventory, customers, reports, users and roles CRUD, permission management, settings, and UI refinement.
user-invocable: true
---

# POS Admin Dashboard — Nuxt 4 + Tailwind CSS + Pinia

## Mission

Build and maintain a modern, responsive, bug-free POS administration interface using Nuxt 4, Vue 3, Tailwind CSS, and Pinia.

**Default behavior: frontend only.**

If no project exists, initialize a Nuxt 4 frontend project and configure the required frontend dependencies. Do not automatically create a Laravel backend, database, Docker configuration, CI/CD pipeline, or other infrastructure.

If a project already exists, inspect its structure and continue using its established conventions. Do not recreate or overwrite an existing project.

The dashboard must support realistic retail workflows, accessible interactions, reusable components, centralized `@apply` styling in `main.css`, Apple-style input controls that prevent iOS/iPadOS Safari auto-zoom on focus, a floating left sidebar driven by a `.jsonc` menu configuration with module and view-permission metadata, a users and roles management module with full CRUD and role-based action permissions (`view` / `create` / `edit` / `delete`), a full-screen POS terminal view (with no sidebar and a gear icon dropdown menu featuring an "Exit" item that redirects to the admin dashboard), and strict zero-bug layout rules across mobile, tablet, and PC.

## 1. Project Detection and Automatic Setup

Before implementing features or modifying layouts, determine whether a project already exists.

### Case A: An existing project is available

1. Inspect the repository tree and `package.json`.
2. Inspect `nuxt.config.ts`, the lockfile, Tailwind configuration, and the existing application directory.
3. Identify the installed Nuxt, Vue, Tailwind CSS (v3 vs v4), and Pinia versions.
4. Inspect existing layouts (`app/layouts/default.vue`), `app.vue`, pages, components, composables, stores, configs, API services, and `main.css`.
5. Preserve working code, `<slot />` outlets, and unrelated user changes.
6. Reuse existing dependencies and conventions.
7. Implement the requested layout or feature modification without breaking existing routes or state.

Do not reinstall dependencies or recreate configuration files unless required.

### Case B: The directory is empty or no Nuxt project exists

Automatically initialize a frontend-only Nuxt 4 project.

Do not stop at providing setup instructions. Execute the available project initialization commands when the environment permits.

Recommended initialization:

```bash
npm create nuxt@latest pos-admin-dashboard
```

If the current working directory is already the intended project directory, initialize there rather than creating a nested project unnecessarily. Use the official Nuxt initialization options supported by the available CLI.

Install and configure:

- Nuxt 4.
- Vue 3 and TypeScript support.
- Tailwind CSS using the integration compatible with the selected Tailwind version.
- Pinia and the appropriate Nuxt integration.
- Only the additional libraries required for the requested features.

Use the package manager indicated by the existing lockfile. For a new project, use npm unless the user specifies another package manager.

After initialization:

1. Verify the generated project structure.
2. Confirm the Nuxt and Tailwind CSS versions.
3. Configure Tailwind CSS and Pinia.
4. Set up `app/assets/css/main.css` with all component, layout, POS full-screen, gear dropdown, and Apple-style input classes defined via `@apply`.
5. Create `app/config/menu.jsonc` with the menu structure, module names, and view permissions.
6. Create the application layout with the floating left sidebar for admin pages, full-screen sidebar-free layout for the POS screen, and gear icon dropdown menu with Exit to admin.
7. Implement the dashboard using realistic demo data.
8. Add feature pages and stores as required.
9. Verify responsive layouts on mobile, tablet, and PC viewports, including Safari focus no-zoom behavior.
10. Run the available type-check, lint, and production-build commands.

If command execution or package installation is unavailable, create the required source and configuration files where possible, then clearly report what could not be executed.

### Strict setup boundaries

Unless explicitly requested, do not:

- Create or install a Laravel backend.
- Create a database or database migrations.
- Configure Docker or Kubernetes.
- Configure Nginx, a production server, or CI/CD.
- Generate backend controllers, models, or API routes.
- Introduce Bootstrap into this Tailwind project.
- Scatter inline Tailwind utility strings across Vue templates when they belong in `main.css` via `@apply`.
- Install unnecessary component libraries or dependencies.
- Replace an existing project with a new scaffold.

The frontend should run independently with clearly identified demo data until a real API is configured.

## 2. Preferred Technology Stack

Use these defaults for new projects:

- Nuxt 4.
- Vue 3 Composition API.
- TypeScript.
- Tailwind CSS with all semantic classes centralized in `app/assets/css/main.css` using `@apply`.
- JSONC (`app/config/menu.jsonc`) for declarative sidebar menu configuration with module and permission definitions.
- Pinia for shared client-side state.
- Nuxt file-based routing.
- Composables for feature-level application logic (including menu parsing and permission filtering).
- Service modules for API communication when needed.
- Laravel as a future or existing API provider, not as part of automatic setup.

Follow the actual versions and conventions in the repository. Do not copy configuration syntax from an incompatible major version.

## 3. MVC-Inspired Frontend Architecture

Use an MVC-inspired structure adapted to Nuxt and Vue.

### Model / Domain

Responsible for:

- TypeScript interfaces and types (including menu items, module identifiers, and permission rules).
- Product, order, customer, inventory, sales, user, and role domain definitions.
- Enums and domain-specific formatting helpers.
- Pure business rules that can safely run on the client.

Do not treat client-side financial, inventory, or permission checks as authoritative security boundaries.

### View

Responsible for:

- Nuxt pages.
- Layouts (floating sidebar shell for admin routes; full-screen sidebar-free shell with gear dropdown for `/pos`).
- Reusable Vue components using clean semantic class names defined in `main.css`.
- Rendering data, displaying errors, and emitting user actions.

Keep complex workflows and long Tailwind utility chains out of page templates.

### Controller / Application Layer

Use composables or feature-level action modules to coordinate:

- User interactions.
- Menu loading from `menu.jsonc` and filtering by module and view permission.
- Form validation.
- API requests.
- Store updates.
- Loading and error states.

### Store

Use Pinia for shared state and workflows across components or routes:

- Cart and checkout UI state.
- Active register session when applicable.
- Authentication, active user role, and granted permissions list (e.g., `permissions: string[]`).
- Users and roles collections for the CRUD module (items, pagination, filters, loading/saving flags, last error).
- Sidebar collapse/drawer state and user preferences.
- Shared product or order data when caching is useful.

### Service / API Layer

Responsible for:

- HTTP communication, request/response types, consistent error handling, and API operation definitions.
- Separate demo-data modules and feature services when no backend exists. Avoid embedding mock data directly in large page components.

## 4. Recommended Project Structure

Adapt the structure to the actual Nuxt 4 conventions and generated scaffold:

```text
app/
  assets/
    css/
      main.css
  config/
    menu.jsonc
  components/
    layout/
      AppSidebar.vue
      AppTopbar.vue
    shared/
      StatCard.vue
      StatusBadge.vue
      DataTable.vue
      EmptyState.vue
      LoadingState.vue
      ErrorState.vue
    dashboard/
    products/
    sales/
    inventory/
    users/
    roles/
      PermissionMatrix.vue
  composables/
    useMenu.ts
    usePermissions.ts
    useProducts.ts
    useSales.ts
    useUsers.ts
    useRoles.ts
  layouts/
    default.vue
  pages/
    index.vue
    pos/
      index.vue
    sales/
      index.vue
    orders/
      index.vue
    products/
      index.vue
    categories/
      index.vue
    inventory/
      index.vue
    customers/
      index.vue
    users/
      index.vue
      new.vue
      [id].vue
    roles/
      index.vue
      new.vue
      [id].vue
    reports/
      index.vue
    settings/
      index.vue
  stores/
    auth.ts
    cart.ts
    preferences.ts
    users.ts
    roles.ts
  services/
    api/
      client.ts
    users.ts
    roles.ts
    demo/
      products.ts
      sales.ts
      users.ts
      roles.ts
  types/
    menu.ts
    product.ts
    sale.ts
    user.ts
    role.ts
  utils/
    jsonc.ts
    currency.ts
    dates.ts
nuxt.config.ts
package.json
```

## 5. Pinia State Management

1. Define typed state, getters, and actions.
2. Keep temporary UI state local to the relevant component.
3. Avoid duplicating the same state across pages, composables, and stores.
4. Do not store API secrets, passwords, or sensitive payment data in Pinia.
5. Treat persisted client state as untrusted.
6. Handle loading, errors, and stale data explicitly.
7. Follow Nuxt SSR-safe state-management patterns.
8. Keep API transport logic in a service layer where appropriate.
9. Never treat client-side prices, discounts, totals, inventory, or permissions as authoritative on the backend.
10. Update state from confirmed operation results, not merely from button clicks.

## 6. Visual Design Direction, Floating Left Sidebar, POS Full-Screen Mode, and Zero-Bug Layout Protocol

Build a polished retail back-office interface using a restrained light theme by default unless the project already has a defined design system.

### Floating Left Sidebar Layout (Admin Pages)

- Design the left sidebar (`AppSidebar.vue`) in a **floating style**: detached from the viewport edges with outer spacing (`top-3 bottom-3 left-3`), rounded corners (`rounded-2xl`), a subtle border, and a soft elevation shadow.
- Support smooth collapsing into a compact floating icon rail on PC/tablet and an off-canvas floating drawer with a backdrop overlay on mobile and tablet portrait screens.
- Keep an internal scrollable navigation list (`overflow-y-auto overscroll-contain`) inside the floating sidebar while pinning the brand header at the top and user/role summary or collapse toggle at the bottom.
- Pair the floating sidebar with a responsive main content wrapper that adjusts its left offset according to the sidebar state on PC and uses full width on mobile.

### POS Screen Mode: No Sidebar & Gear Icon Dropdown with Exit to Admin

When accessing the POS screen (`/pos` or `/pos/**`):

1. **No Sidebar on POS Screen:**
   - Do **not** render `AppSidebar.vue` or `.sidebar-backdrop` on the POS screen.
   - Remove the desktop sidebar left padding offset (`lg:pl-72` / `lg:pl-28`) by applying `.main-viewport-pos` (`@apply lg:pl-6;`) so the POS terminal utilizes 100% of the viewport width across mobile, tablet, and PC.
2. **Gear Icon Dropdown Menu with "Exit" Redirect to Admin:**
   - Render a **Gear Icon button** (`.pos-gear-btn`) in the top-right corner of `AppTopbar.vue` when on the POS screen.
   - Clicking the gear icon toggles a floating dropdown menu (`.pos-gear-dropdown`) with `aria-expanded` and `aria-haspopup="menu"`.
   - Inside the dropdown menu, include an **"Exit"** item (`.pos-gear-dropdown-exit`).
   - Clicking **"Exit"** closes the dropdown and redirects the user to the admin dashboard via `await navigateTo('/')` (or `/admin` if the project uses `/admin`).
   - Automatically close the gear dropdown on outside click, `Escape` key press, or route change.

### Critical Layout Modification & Zero-Bug Safeguards

1. **Exact Geometry & Offset Synchronization:**
   - Admin Expanded floating sidebar: `left-3` (`0.75rem`) + `w-64` (`16rem`) = `16.75rem` occupied width. `.main-viewport` MUST use `lg:pl-72` (`18rem`) for a clean `1.25rem` gap and zero overlap.
   - Admin Collapsed floating sidebar: `left-3` (`0.75rem`) + `lg:w-20` (`5rem`) = `5.75rem` occupied width. `.main-viewport-collapsed` MUST use `lg:pl-28` (`7rem`).
   - Mobile Hidden State: Use `-translate-x-[calc(100%+1.5rem)]` so the `left-3` offset and shadow never bleed onto mobile screens when closed.
   - POS Screen (No Sidebar): `.main-viewport-pos` MUST override the left offset with `lg:pl-6` (`1.5rem`) so there is zero empty gutter on the left side.
2. **Prevent Flex & Table Horizontal Overflow (`min-w-0`):**
   - Always apply `min-w-0 flex-1 overflow-x-hidden` on `.main-viewport` and `.page-content`.
3. **Strict Z-Index Layering Scale:**
   - Sticky Topbar: `z-20`
   - Mobile Sidebar Backdrop Overlay: `z-30`
   - Floating Left Sidebar (`AppSidebar.vue`): `z-40`
   - POS Gear Dropdown Menu & Modals/Dialogs: `z-50`
4. **SSR Hydration Safety:**
   - Initialize layout state via `useState` with deterministic SSR defaults (`false`) instead of reading `window.innerWidth` during SSR setup.

### Reference Layout Implementation (`app/layouts/default.vue` & `app/components/layout/AppTopbar.vue`)

```vue
<!-- app/layouts/default.vue -->
<script setup lang="ts">
const route = useRoute()

const isSidebarCollapsed = useState<boolean>('pos-sidebar-collapsed', () => false)
const isMobileMenuOpen = useState<boolean>('pos-mobile-menu-open', () => false)

const isPosScreen = computed(() => {
  return route.path === '/pos' || route.path.startsWith('/pos/')
})

function toggleDesktopSidebar() {
  isSidebarCollapsed.value = !isSidebarCollapsed.value
}

function openMobileMenu() {
  isMobileMenuOpen.value = true
}

function closeMobileMenu() {
  isMobileMenuOpen.value = false
}

watch(
  () => route.fullPath,
  () => {
    isMobileMenuOpen.value = false
  }
)
</script>

<template>
  <div class="app-shell">
    <template v-if="!isPosScreen">
      <div
        v-if="isMobileMenuOpen"
        class="sidebar-backdrop"
        aria-hidden="true"
        @click="closeMobileMenu"
      />

      <LayoutAppSidebar :collapsed="isSidebarCollapsed" :mobile-open="isMobileMenuOpen" @close-mobile="closeMobileMenu" @toggle-collapse="toggleDesktopSidebar"/>
    </template>

    <div
      class="main-viewport"
      :class="{
        'main-viewport-collapsed': !isPosScreen && isSidebarCollapsed,
        'main-viewport-pos': isPosScreen
      }"
    >
      <LayoutAppTopbar :is-pos-screen="isPosScreen" :sidebar-collapsed="isSidebarCollapsed" @open-mobile-menu="openMobileMenu" @toggle-sidebar="toggleDesktopSidebar"/>

      <main class="page-content">
        <slot />
      </main>
    </div>
  </div>
</template>
```

```vue
<!-- app/components/layout/AppTopbar.vue -->
<script setup lang="ts">
const props = defineProps<{
  isPosScreen?: boolean
  sidebarCollapsed?: boolean
}>()

const emit = defineEmits<{
  openMobileMenu: []
  toggleSidebar: []
}>()

const route = useRoute()
const isGearOpen = ref(false)
const gearWrapperRef = ref<HTMLElement null |>(null)

function toggleGearMenu() {
  isGearOpen.value = !isGearOpen.value
}

function closeGearMenu() {
  isGearOpen.value = false
}

async function exitPosToAdmin() {
  closeGearMenu()
  await navigateTo('/')
}

watch(
  () => route.fullPath,
  () => {
    closeGearMenu()
  }
)

onMounted(() => {
  const handleClickOutside = (event: MouseEvent) => {
    if (
      isGearOpen.value &&
      gearWrapperRef.value &&
      !gearWrapperRef.value.contains(event.target as Node)
    ) {
      closeGearMenu()
    }
  }

  const handleKeyDown = (event: KeyboardEvent) => {
    if (event.key === 'Escape' && isGearOpen.value) {
      closeGearMenu()
    }
  }

  document.addEventListener('mousedown', handleClickOutside)
  window.addEventListener('keydown', handleKeyDown)

  onUnmounted(() => {
    document.removeEventListener('mousedown', handleClickOutside)
    window.removeEventListener('keydown', handleKeyDown)
  })
})
</script>

<template>
  <header class="topbar">
    <div class="flex items-center gap-3">
      <button
        v-if="!props.isPosScreen"
        type="button"
        class="btn-icon lg:hidden"
        aria-label="Open navigation menu"
        @click="emit('openMobileMenu')"
      >
        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
        </svg>
      </button>

      <h1 class="text-base font-semibold text-slate-900">
        {{ props.isPosScreen ? 'Point of Sale Terminal' : 'POS Admin' }}
      </h1>
    </div>

    <div class="flex items-center gap-2">
      <div v-if="props.isPosScreen" ref="gearWrapperRef" class="pos-gear-wrapper">
        <button
          type="button"
          class="pos-gear-btn"
          aria-label="POS Settings Menu"
          aria-haspopup="menu"
          :aria-expanded="isGearOpen"
          @click="toggleGearMenu"
        >
          <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M10.325 4.317c.426-1.756 2.924-1.756 3.35 0a1.724 1.724 0 002.573 1.066c1.543-.94 3.31.826 2.37 2.37a1.724 1.724 0 001.065 2.572c1.756.426 1.756 2.924 0 3.35a1.724 1.724 0 00-1.066 2.573c.94 1.543-.826 3.31-2.37 2.37a1.724 1.724 0 00-2.572 1.065c-.426 1.756-2.924 1.756-3.35 0a1.724 1.724 0 00-2.573-1.066c-1.543.94-3.31-.826-2.37-2.37a1.724 1.724 0 00-1.065-2.572c-1.756-.426-1.756-2.924 0-3.35a1.724 1.724 0 001.066-2.573c-.94-1.543.826-3.31 2.37-2.37.996.608 2.296.07 2.572-1.065z"
            />
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" />
          </svg>
        </button>

        <div
          v-if="isGearOpen"
          class="pos-gear-dropdown"
          role="menu"
          aria-orientation="vertical"
        >
          <button
            type="button"
            class="pos-gear-dropdown-exit"
            role="menuitem"
            @click="exitPosToAdmin"
          >
            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 16l4-4m0 0l-4-4m4 4H7m6 4v1a3 3 0 01-3 3H6a3 3 0 01-3-3V7a3 3 0 013-3h4a3 3 0 013 3v1" />
            </svg>
            <span>Exit</span>
          </button>
        </div>
      </div>
    </div>
  </header>
</template>
```

## 7. JSONC Menu Configuration, Modules, and View Permissions

Define the sidebar navigation in `app/config/menu.jsonc` (JSON with Comments). Every menu entry must specify its `module` name and `permission.view` key.

### JSONC Menu Structure (`app/config/menu.jsonc`)

```jsonc
{
  // POS Admin Dashboard Navigation Configuration
  "groups": [
    {
      "groupTitle": "Core Operations",
      "items": [
        {
          "id": "overview",
          "label": "Overview",
          "icon": "layout-dashboard",
          "route": "/",
          "module": "dashboard",
          "permission": { "view": "dashboard.view" }
        },
        {
          "id": "pos",
          "label": "Point of Sale",
          "icon": "calculator",
          "route": "/pos",
          "module": "pos",
          "permission": { "view": "pos.view" }
        },
        {
          "id": "sales",
          "label": "Sales",
          "icon": "receipt",
          "route": "/sales",
          "module": "sales",
          "permission": { "view": "sales.view" }
        },
        {
          "id": "orders",
          "label": "Orders & Returns",
          "icon": "shopping-bag",
          "route": "/orders",
          "module": "orders",
          "permission": { "view": "orders.view" }
        }
      ]
    },
    {
      "groupTitle": "Catalog & Stock",
      "items": [
        {
          "id": "products",
          "label": "Products",
          "icon": "package",
          "route": "/products",
          "module": "products",
          "permission": { "view": "products.view" }
        },
        {
          "id": "categories",
          "label": "Categories",
          "icon": "tags",
          "route": "/categories",
          "module": "categories",
          "permission": { "view": "categories.view" }
        },
        {
          "id": "inventory",
          "label": "Inventory",
          "icon": "warehouse",
          "route": "/inventory",
          "module": "inventory",
          "permission": { "view": "inventory.view" }
        }
      ]
    },
    {
      "groupTitle": "Management",
      "items": [
        {
          "id": "customers",
          "label": "Customers",
          "icon": "users",
          "route": "/customers",
          "module": "customers",
          "permission": { "view": "customers.view" }
        },
        {
          "id": "users",
          "label": "Users",
          "icon": "user-cog",
          "route": "/users",
          "module": "users",
          "permission": {
            "view": "users.view",
            "create": "users.create",
            "edit": "users.edit",
            "delete": "users.delete"
          }
        },
        {
          "id": "roles",
          "label": "Roles & Permissions",
          "icon": "shield-check",
          "route": "/roles",
          "module": "roles",
          "permission": {
            "view": "roles.view",
            "create": "roles.create",
            "edit": "roles.edit",
            "delete": "roles.delete"
          }
        },
        {
          "id": "reports",
          "label": "Reports",
          "icon": "bar-chart-3",
          "route": "/reports",
          "module": "reports",
          "permission": { "view": "reports.view" }
        },
        {
          "id": "settings",
          "label": "Settings",
          "icon": "settings",
          "route": "/settings",
          "module": "settings",
          "permission": { "view": "settings.view" }
        }
      ]
    }
  ]
}
```

### Safe JSONC Parser (`app/utils/jsonc.ts`)

```ts
export function parseJsonc<T>(rawJsonc: string): T {
  const stripped = rawJsonc.replace(
    /\\"|"(?:\\"|[^"])*"|(\/\/.*|\/\*[\s\S]*?\*\/)/g,
    (match, commentGroup) => (commentGroup ? '' : match)
  )
  return JSON.parse(stripped) as T
}
```

### Menu and Permission Requirements

- Parse `menu.jsonc` safely in `app/utils/jsonc.ts` without corrupting strings or URLs.
- Type every menu item in `app/types/menu.ts` with `id`, `label`, `icon`, `route`, `module`, and `permission: { view: string; create?: string; edit?: string; delete?: string }`.
- Filter menu items and groups in `useMenu.ts` using `usePermissions.ts` so users only see modules where they hold `permission.view` (or `"*"` for super admin).
- Only `permission.view` affects sidebar visibility and route access. The optional `create`, `edit`, and `delete` keys gate primary buttons, row actions, and form submission inside the module (see section 10).
- Hide any group whose items are all filtered out.

## 8. Dashboard Overview

Include gross sales, net sales, transaction count, average order value, low-stock alerts, sales trend chart, recent transactions, best-selling products, and quick actions filtered by module view permissions. Support loading, empty, error, and retry states with clearly identified demo data when no API is configured.

## 9. Tables and Management Pages

Use a consistent pattern for products, sales, orders, inventory, customers, users, and roles: page heading, module badge, primary action, search/filters, readable columns, status badges, pagination, row actions, loading/empty/error states, and destructive action confirmation.

## 10. Users & Roles Module (CRUD with Role-Based Permissions)

The `users` and `roles` modules manage staff accounts and the roles that grant the permission keys consumed by `menu.jsonc`, `usePermissions.ts`, route guards, and action buttons. Both modules must support full Create, Read, Update, and Delete workflows in demo mode first, using a service contract that can later be pointed at Laravel without touching the pages.

If an existing project already ships a `staff` module, evolve it in place (keep its route, add the roles page and the CRUD actions below). Do not create a duplicate module.

### Permission Keys (`<module>.<action>`)

| Module  | view         | create         | edit         | delete         |
|---------|--------------|----------------|--------------|----------------|
| `users` | `users.view` | `users.create` | `users.edit` | `users.delete` |
| `roles` | `roles.view` | `roles.create` | `roles.edit` | `roles.delete` |

- `permission.view` controls sidebar visibility and route access. `create`, `edit`, and `delete` gate the primary button, row actions, and form submission.
- Register all eight keys in `menu.jsonc` (see the `users` and `roles` entries in section 7) and in the demo role profiles of `auth.ts`.
- Support a `"*"` wildcard for super administrators inside `hasPermission`.
- Expose `can(action, module)` from `usePermissions.ts` as a thin wrapper around ``hasPermission(`${module}.${action}`)``.
- Frontend checks are presentation only. Laravel must enforce the same keys on every endpoint.

### Domain Types (`app/types/user.ts`, `app/types/role.ts`)

```ts
// app/types/user.ts
export type UserStatus = 'active' | 'inactive' | 'suspended'

export interface User {
  id: string
  name: string
  email: string
  phone?: string
  avatar?: string
  roleId: string
  status: UserStatus
  registerId?: string
  createdAt: string
  updatedAt: string
  lastLoginAt?: string
}

export interface UserFormInput {
  name: string
  email: string
  phone?: string
  roleId: string
  status: UserStatus
  password?: string             // create, or explicit "Reset password" only
  passwordConfirmation?: string
}

// app/types/role.ts
export interface Role {
  id: string
  name: string
  slug: string
  description?: string
  permissions: string[]         // e.g. ['products.view', 'products.edit', 'roles.view']
  isSystem: boolean             // system roles (e.g. admin) cannot be deleted or renamed
  usersCount: number
  createdAt: string
  updatedAt: string
}

export interface RoleFormInput {
  name: string
  description?: string
  permissions: string[]
}
```

### Service Contract (`app/services/users.ts`, `app/services/roles.ts`)

Both services expose the same shape so the demo implementation can be swapped for Laravel without changing pages or stores:

```ts
export interface ListParams {
  search?: string
  page?: number
  perPage?: number
  roleId?: string
  status?: UserStatus
}

export interface Paginated<T> {
  data: T[]
  total: number
  page: number
  perPage: number
}

export interface UserService {
  list(params: ListParams): Promise<Paginated<User>>
  get(id: string): Promise<User>
  create(input: UserFormInput): Promise<User>
  update(id: string, input: UserFormInput): Promise<User>
  remove(id: string): Promise<void>
}
// RoleService mirrors UserService with Role / RoleFormInput.
```

- Demo mode: `app/services/demo/users.ts` and `app/services/demo/roles.ts` keep an in-memory fixture list, simulate short latency, enforce unique emails and role names, and reject invalid input with the same shape Laravel returns (`{ message, errors: { field: ['...'] } }`, status `422`).
- Laravel mode (later): `GET/POST /users`, `GET/PUT/DELETE /users/{id}`, `GET/POST /roles`, `GET/PUT/DELETE /roles/{id}`, and `GET /permissions` for the permission catalog. Verify the real routes and response envelopes first; never invent endpoints.

### Stores (`app/stores/users.ts`, `app/stores/roles.ts`)

- State: `items`, `total`, `page`, `perPage`, `filters`, `isLoading`, `isSaving`, `error`.
- Actions: `fetch(params)`, `create(input)`, `update(id, input)`, `remove(id)`.
- Update `items` only from the confirmed service result, never optimistically on click.
- Never keep passwords in state. Clear password fields in the form after submit.
- When the edited user is the signed-in user, refresh the permissions in `auth.ts` from the confirmed response so the sidebar re-filters immediately.

### Pages and Routes

```text
pages/users/index.vue   # list + search / filters + pagination + row actions
pages/users/new.vue     # create form
pages/users/[id].vue    # edit form (+ Delete through a confirmation dialog)
pages/roles/index.vue   # list with users count + permissions count
pages/roles/new.vue     # create form + permission matrix
pages/roles/[id].vue    # edit form + permission matrix (+ Delete)
```

Guard every route with the module's `permission.view` using the same gate as the other modules.

**Users list (`/users`)**

- Columns: avatar + name + email, role badge, status badge (text label, never color only), last login, actions.
- Filters: `.apple-search-input` (name / email / phone), role `.apple-select`, status `.apple-select`. Keep them synchronized with URL query parameters.
- "New User" is shown only with `users.create`; Edit / Delete row actions only with `users.edit` / `users.delete`.
- Loading, empty, no-results, and error states with retry. Pagination with result counts.

**User form (`/users/new`, `/users/[id]`)**

- `.form-label` + `.apple-input` for name, email (`type="email"`), phone (`type="tel"`, `inputmode="tel"`); role and status use `.apple-select`.
- Password and confirmation appear only on create or after an explicit "Reset password" toggle, with `autocomplete="new-password"`.
- Validate on submit, show `.form-error` beside the field, preserve entered values, disable the submit button while `isSaving`, and prevent duplicate requests.
- Success and failure feedback through the toast store; redirect to `/users` only after a confirmed save.

**Roles list and form (`/roles`, `/roles/new`, `/roles/[id]`)**

- Columns: role name, description, permissions count, users count, system badge, actions.
- The permission matrix is generated from `menu.jsonc` (one row per module, one checkbox per action) so new modules appear automatically. Render it as `components/roles/PermissionMatrix.vue` with the `.permission-matrix` and `.apple-checkbox` classes from `main.css`.
- Provide "select all" per row and per column and a live count of granted permissions.
- System roles (`isSystem: true`) can have their permissions reviewed but not be renamed or deleted.

### Permission Matrix Component (`app/components/roles/PermissionMatrix.vue`)

```ts
// Rows come from menu.jsonc so the matrix never drifts from the sidebar modules.
const ACTIONS = ['view', 'create', 'edit', 'delete'] as const
const { rawMenu } = useMenu()

const rows = computed(() =>
  rawMenu.groups.flatMap(group =>
    group.items.map(item => ({
      module: item.module,
      label: item.label,
      keys: ACTIONS.map(action => item.permission[action] ?? `${item.module}.${action}`)
    }))
  )
)
// v-model is the role's `permissions: string[]`; toggling a checkbox adds/removes its key.
```

### Delete and Safety Rules

- Every delete goes through a confirmation dialog (`.modal-backdrop` / `.modal-card`) that names the record being removed.
- Block in the UI, and explain why, when the user tries to: delete their own account, delete or deactivate the last active user who holds `users.edit`, delete a system role, or delete a role that still has assigned users (show the count and link to the filtered users list).
- Prefer deactivation (`status: 'inactive'`) as the day-to-day action; hard delete stays gated by `users.delete`.
- Role and permission changes take effect in the UI only after the server (or demo service) confirms them.

### Reference `main.css` Additions

```css
@layer components {
  .permission-matrix {
    @apply w-full text-left border-collapse min-w-[560px];
  }

  .permission-matrix th {
    @apply px-3 py-2.5 text-xs font-semibold uppercase tracking-wider text-slate-500
           bg-slate-50/90 border-b border-slate-200/80 text-center first:text-left;
  }

  .permission-matrix td {
    @apply px-3 py-2.5 text-sm text-slate-700 border-b border-slate-100 text-center
           first:text-left first:font-semibold;
  }

  .apple-checkbox {
    @apply h-5 w-5 rounded-md accent-blue-600 cursor-pointer
           focus-visible:outline-none focus-visible:ring-4 focus-visible:ring-blue-500/15;
  }
}
```

Keep native rendering for checkboxes and radios: scope the global `appearance-none` base rule to text-like controls (`input:not([type="checkbox"]):not([type="radio"])`) so the matrix stays usable and accessible.

### Demo Role Profiles (`auth.ts`)

- `admin`: every `users.*` and `roles.*` key (or `"*"`).
- `manager`: `users.view`, `users.create`, `users.edit`, `roles.view`.
- `cashier` and `inventory_clerk`: none of the users / roles keys.
- Add the `manager` profile to the sidebar role switcher so partial CRUD gating can be tested without a backend.

### Acceptance Checklist

- `/users` and `/roles` appear in the sidebar only with `users.view` / `roles.view`; direct navigation without the key shows the Permission Denied state.
- Create, edit, and delete work end to end in demo mode with validation messages, confirmation dialogs, and toast feedback.
- Buttons and row actions honor `create` / `edit` / `delete` keys for the active simulated role.
- Permission matrix rows match the modules in `menu.jsonc` exactly.
- Deleting self, the last `users.edit` holder, a system role, or an in-use role is blocked with an explanation.
- All form controls compute to `16px` below `1024px` (Safari no-zoom) and use `.apple-*` classes; tables scroll inside `.data-table-wrapper` on mobile.

## 11. Forms, Apple-Style Input Boxes, and Safari Focus No-Zoom Behavior

### Apple-Style Input Design & Safari Auto-Zoom Prevention

- **Prevent iOS / iPadOS Safari Auto-Zoom on Focus:** Safari on iPhone and iPad automatically zooms the viewport whenever a user focuses an `<input>`, `<select>`, or `<textarea>` with a computed `font-size` below `16px`. Always enforce `text-base` (`16px` / `1rem`) on all form controls across mobile and tablet viewports (`@apply text-base lg:text-sm`). Never use `text-xs` or unprefixed `text-sm` on any input, search box, POS quantity input, or select element.
- **Apple-Inspired Input Aesthetics (`.apple-input` / `.form-input`):**
  - Strip native WebKit inner shadows using `appearance-none` (`-webkit-appearance: none`).
  - Use smooth Apple-like geometry: `rounded-xl`, `min-h-[44px]`, `bg-slate-100/80`, `border border-slate-200/90`, and `focus:bg-white focus:border-blue-500 focus:ring-4 focus:ring-blue-500/15 focus:outline-none`.
  - Apply `touch-action: manipulation` to prevent accidental double-tap zoom while preserving pinch-to-zoom accessibility.

## 12. Nuxt 4 Conventions

Use `<script setup lang="ts">`, file-based routing, reusable components, typed contracts, `useFetch`/`useAsyncData`, SSR-safe patterns, `runtimeConfig.public.apiBase`, and semantic HTML with accessible labels.

## 13. Tailwind CSS Conventions (`main.css` with `@apply`)

**Mandatory Rule: Centralize all Tailwind CSS classes in `app/assets/css/main.css` using `@apply`.**

- Do not clutter Vue `<template>` blocks with long utility class chains; use semantic class names defined in `app/assets/css/main.css`.
- Never `@apply` one custom component class inside another custom class in Tailwind v4.

Reference pattern in `app/assets/css/main.css`:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  html,
  body {
    @apply bg-slate-100 text-slate-900 antialiased overflow-x-hidden;
  }

  /* Prevent iOS & iPadOS Safari from auto-zooming on input focus (must be >= 16px on mobile/tablet) */
  input,
  select,
  textarea {
    @apply text-base lg:text-sm appearance-none;
    touch-action: manipulation;
    -webkit-appearance: none;
  }
}

@layer components {
  .app-shell {
    @apply relative min-h-dvh w-full bg-slate-100 text-slate-900 flex flex-col overflow-x-hidden;
  }

  .sidebar-backdrop {
    @apply fixed inset-0 z-30 bg-slate-900/40 backdrop-blur-sm transition-opacity duration-300 lg:hidden;
  }

  .sidebar-float {
    @apply fixed top-3 bottom-3 left-3 z-40 w-64 rounded-2xl bg-white/95 backdrop-blur-md border border-slate-200/80 shadow-xl flex flex-col overflow-hidden transition-all duration-300 ease-in-out -translate-x-[calc(100%+1.5rem)] lg:translate-x-0;
  }

  .sidebar-float-open {
    @apply translate-x-0;
  }

  .sidebar-float-collapsed {
    @apply lg:w-20;
  }

  .sidebar-header {
    @apply flex items-center justify-between gap-3 px-4 h-16 border-b border-slate-100 shrink-0;
  }

  .sidebar-scroll-area {
    @apply flex-1 overflow-y-auto overscroll-contain px-3 py-4 space-y-5;
  }

  .sidebar-footer {
    @apply p-3 border-t border-slate-100 shrink-0 bg-slate-50/60;
  }

  .sidebar-group-title {
    @apply px-3 mb-1.5 text-[11px] font-semibold uppercase tracking-wider text-slate-400 truncate;
  }

  .sidebar-link {
    @apply flex items-center gap-3 px-3 py-2.5 min-h-[44px] rounded-xl text-sm font-medium text-slate-600 hover:bg-slate-100/90 hover:text-slate-900 transition-colors duration-150 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-blue-500;
  }

  .sidebar-link-active {
    @apply bg-blue-600 text-white shadow-sm hover:bg-blue-600 hover:text-white;
  }

  .main-viewport {
    @apply flex-1 flex flex-col min-w-0 w-full px-3 py-3 sm:px-5 md:px-6 md:py-4 lg:pl-72 transition-all duration-300 ease-in-out;
  }

  .main-viewport-collapsed {
    @apply lg:pl-28;
  }

  .main-viewport-pos {
    @apply lg:pl-6;
  }

  .topbar {
    @apply sticky top-3 z-20 flex items-center justify-between gap-3 px-4 h-16 rounded-2xl bg-white/90 backdrop-blur-md border border-slate-200/80 shadow-sm mb-4 shrink-0;
  }

  .pos-gear-wrapper {
    @apply relative inline-block text-left;
  }

  .pos-gear-btn {
    @apply inline-flex items-center justify-center min-w-[44px] min-h-[44px] rounded-xl bg-slate-100/90 text-slate-700 hover:bg-slate-200/80 hover:text-slate-900 transition-colors duration-150 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-blue-500;
  }

  .pos-gear-dropdown {
    @apply absolute right-0 mt-2 w-48 origin-top-right rounded-2xl bg-white p-1.5 shadow-xl border border-slate-200/90 z-50 focus:outline-none;
  }

  .pos-gear-dropdown-item {
    @apply flex w-full items-center gap-2.5 px-3 py-2.5 min-h-[44px] rounded-xl text-sm font-medium text-slate-700 hover:bg-slate-100 transition-colors duration-150;
  }

  .pos-gear-dropdown-exit {
    @apply flex w-full items-center gap-2.5 px-3 py-2.5 min-h-[44px] rounded-xl text-sm font-medium text-rose-600 hover:bg-rose-50 transition-colors duration-150;
  }

  .page-content {
    @apply flex-1 min-w-0 space-y-6 pb-8;
  }

  .apple-input,
  .form-input {
    @apply w-full min-h-[44px] px-3.5 py-2.5 rounded-xl bg-slate-100/80 border border-slate-200/90 text-base lg:text-sm text-slate-900 placeholder:text-slate-400 appearance-none shadow-sm transition-all duration-200 focus:bg-white focus:border-blue-500 focus:ring-4 focus:ring-blue-500/15 focus:outline-none disabled:opacity-50 disabled:cursor-not-allowed;
    touch-action: manipulation;
    -webkit-appearance: none;
  }

  .apple-search-input {
    @apply w-full min-h-[44px] pl-10 pr-4 py-2 rounded-xl bg-slate-100/90 border border-transparent text-base lg:text-sm text-slate-900 placeholder:text-slate-400 appearance-none transition-all duration-200 focus:bg-white focus:border-blue-500 focus:ring-4 focus:ring-blue-500/15 focus:outline-none;
    touch-action: manipulation;
    -webkit-appearance: none;
  }

  .stats-grid {
    @apply grid grid-cols-1 gap-4 sm:grid-cols-2 xl:grid-cols-4;
  }

  .table-container {
    @apply w-full overflow-x-auto rounded-2xl bg-white border border-slate-200/80 shadow-sm;
  }
}
```

## 14. Laravel API Integration — Optional

Verify that a Laravel API exists or is requested before integrating. Map backend user permissions to the `permission.view` keys (and the `create` / `edit` / `delete` action keys) in `app/config/menu.jsonc`, replace the demo users / roles services with API-backed ones that keep the same contract, keep demo fixtures separate from live API services, and handle validation and auth errors explicitly.

## 15. POS-Specific Correctness

Keep order, payment, and fulfillment statuses distinct. Distinguish stock on hand, reserved stock, and available stock. Enforce module and action permissions both in the UI (`menu.jsonc` + route guards) and on the backend.

## 16. Accessibility and Responsive Behavior (Mobile, Tablet, and PC)

### 1. Mobile (`< 768px`)

- **Floating Sidebar (Admin Routes):** Hidden off-screen (`-translate-x-[calc(100%+1.5rem)]`) by default; opens as a floating rounded drawer over `.sidebar-backdrop`. Closes on route change, `Escape`, or backdrop tap.
- **POS Screen (No Sidebar + Gear Dropdown):** Full-width layout with no sidebar. Topbar displays `.pos-gear-btn` which opens `.pos-gear-dropdown` containing the **Exit** item to return to Admin (`/`).
- **Safari Input Focus (No Auto-Zoom):** All `.apple-input` and form controls compute to `16px` (`text-base`) so focusing an input on iOS Safari never zooms the screen.
- **Grids & Tables:** Single-column cards (`grid-cols-1`) and horizontal scroll containers (`.table-container`) with `min-w-0` protection.

### 2. Tablet (`768px - 1024px`, `md:`)

- **Floating Sidebar (Admin Routes):** Toggleable floating drawer or collapsed rail (`w-20`).
- **POS Screen (No Sidebar + Gear Dropdown):** Full-width 2-column split or collapsible cart drawer with the Gear Icon dropdown to exit to Admin.
- **iPadOS Safari Input Focus (No Auto-Zoom):** Maintain `text-base` (`16px`) up through tablet breakpoints (`lg:text-sm` only on desktop `>= 1024px`).

### 3. PC / Desktop (`> 1024px`, `lg:` and `xl:`)

- **Floating Sidebar (Admin Routes):** Persistent floating left panel (`lg:translate-x-0`) toggling smoothly between expanded (`w-64` / `lg:pl-72`) and collapsed icon rail (`lg:w-20` / `lg:pl-28`).
- **POS Screen (No Sidebar + Gear Dropdown):** Sidebar hidden and `.main-viewport-pos` resets left padding (`lg:pl-6`) for 100% width. Top-right Gear Icon opens the dropdown with **Exit** redirecting to Admin (`/`).

## 17. Phased Implementation

1. **Phase 1 — Foundation & Bug-Free Layout Shell:** Initialize Nuxt 4, Tailwind CSS, and Pinia; configure `@apply` classes in `app/assets/css/main.css`; create `app/config/menu.jsonc`; build `default.vue`, `AppSidebar.vue`, and `AppTopbar.vue` with POS full-screen detection and the gear dropdown Exit action.
2. **Phase 2 — Dashboard UI:** Build the overview page using semantic classes from `main.css`.
3. **Phase 3 — Core POS Modules:** Build `/pos` (sidebar-free with gear dropdown Exit and `.apple-input` controls), products, categories, sales, and orders.
4. **Phase 4 — Additional Modules:** Inventory, customers, suppliers, the users & roles CRUD module with permission matrix (section 10), reports, and settings.
5. **Phase 5 — API Integration:** Connect to Laravel API and map permissions to `menu.jsonc`.
6. **Phase 6 — Quality & Responsive Refinement:** Audit `@apply` centralization, layout geometry on admin vs POS routes, Safari focus no-zoom (`16px`), mobile/tablet/PC responsiveness, and build/typecheck results.

## 18. Verification and Completion Report

Run configured checks (`npm run dev`, `npm run lint`, `npm run typecheck`, `npm run build`) and report:

1. What changed.
2. Files created or modified (`app/assets/css/main.css`, `app/config/menu.jsonc`, `app/layouts/default.vue`, `app/components/layout/AppTopbar.vue`, and any users / roles CRUD pages, stores, services, and types).
3. Layout & responsive verification across Mobile, Tablet, and PC (including POS no-sidebar mode, gear dropdown Exit redirect, and Safari input no-zoom).
4. Commands executed and actual results.
5. Demo data vs live API status, assumptions, and recommended next step.

## 19. Editing This Skill

Update the relevant section when project requirements change: design tokens, `@apply` classes in `main.css`, `menu.jsonc` modules/permissions, users & roles CRUD rules and permission actions (section 10), POS layout rules, or backend API contracts.

**Final rule:** If no project exists, set up Nuxt 4 and build the frontend only. If a project exists, inspect it and continue from its current state. Keep all Tailwind styling organized in `main.css` with `@apply`, style inputs with Apple-inspired `.apple-input` rules that prevent Safari auto-zoom on focus (`text-base lg:text-sm`), render the left sidebar in a bug-free floating style on admin pages driven by `menu.jsonc` with module names and view permissions, provide full CRUD for users and roles gated by `view` / `create` / `edit` / `delete` permission keys, hide the sidebar on the POS screen with a gear icon dropdown containing an Exit item to return to admin, and verify responsive behavior across mobile, tablet, and PC.