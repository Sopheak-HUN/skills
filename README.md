# Skills

A collection of reusable **agent skills** (`SKILL.md` files) for AI coding assistants such as Claude Code. Each skill is a self-contained instruction set that tells the agent how to design, build, and verify a specific kind of project.

| Skill | What it builds |
|---|---|
| [`pos-admin-dashboard`](pos-admin-dashboard/SKILL.md) | A production-style Point of Sale admin dashboard with Nuxt 4, Tailwind CSS, and Pinia |

## pos-admin-dashboard

`pos-admin-dashboard` turns a one-line request ("build me a POS admin") into a complete, opinionated frontend. It detects whether a Nuxt project already exists, scaffolds one if not, and then builds or extends the dashboard while enforcing a strict set of layout, styling, and correctness rules.

### What the agent produces

- **Floating left sidebar** driven by a `menu.jsonc` config. Every entry declares its `module` name and `permission.view` key, so the sidebar, route guards, and page headers all stay in sync. Collapsible icon rail on PC, slide-over drawer on mobile and tablet.
- **Full-screen POS mode** on `/pos`: no sidebar, full viewport width, and a gear-icon dropdown with an **Exit** item that returns to the admin dashboard. Closes on outside click, `Escape`, or route change.
- **Users & Roles CRUD** with role-based action permissions (`users.view/create/edit/delete`, `roles.view/create/edit/delete`), a permission matrix generated from `menu.jsonc`, Laravel-shaped validation errors, and delete safety rules (no deleting yourself, the last admin, a system role, or a role still in use).
- **Dashboard, POS terminal, products, categories, inventory, sales, orders, customers, reports, and settings** pages built on a consistent table and form pattern with loading, empty, error, and retry states.
- **Apple-style inputs that never trigger iOS Safari auto-zoom.** Every form control computes to at least 16px below 1024px (`text-base lg:text-sm`), with 44px touch targets and `touch-action: manipulation`, without disabling pinch-to-zoom.
- **One select control and rounded checkboxes.** Every picker uses `AppSelect.vue`, a wrapper around `vue-multiselect` 3.x skinned in `main.css` with dark-mode support. Checkboxes are native inputs with a rounded custom style and an indeterminate state for "Select all".
- **Centralized Tailwind styling.** All utility chains live in `app/assets/css/main.css` as `@apply` component classes; Vue templates use semantic class names only.
- **Zero-bug layout protocol.** Exact sidebar/content geometry (`lg:pl-72` expanded, `lg:pl-28` collapsed, `lg:pl-6` on POS), a fixed z-index ladder (topbar 20, backdrop 30, sidebar 40, dropdowns and modals 50), `min-w-0` overflow protection, and SSR-safe layout state.
- **Frontend-only by default.** Demo data is clearly isolated in service modules, and the API layer is shaped so a Laravel backend can be plugged in later without rewriting pages. The skill never creates a backend, database, Docker, or CI config unless asked.

### How the skill works

1. **Project detection.** Inspects `package.json`, the lockfile, `nuxt.config.ts`, and the app directory. Existing projects are extended in place; nothing is scaffolded over them.
2. **Phased build.** Foundation and layout shell, then dashboard, then core POS modules, then management modules, then optional API integration, then a quality pass.
3. **Verification.** Runs the configured type-check, lint, and production build, verifies mobile / tablet / PC behaviour, and reports exactly which commands ran and what they returned.

### Installation

Copy the skill folder into the skills directory your agent reads. For Claude Code, either of these works:

```text
.agents/skills/pos-admin-dashboard/SKILL.md
.claude/skills/pos-admin-dashboard/SKILL.md
```

With the GitHub CLI:

```bash
mkdir -p .agents/skills/pos-admin-dashboard
gh api repos/Sopheak-HUN/skills/contents/pos-admin-dashboard/SKILL.md \
  -H "Accept: application/vnd.github.raw" > .agents/skills/pos-admin-dashboard/SKILL.md
```

Or clone the repo and copy the folder:

```bash
git clone https://github.com/Sopheak-HUN/skills.git
cp -r skills/pos-admin-dashboard .agents/skills/
```

### Usage

Open your project in Claude Code and invoke the skill by name, or mention the file:

```text
/pos-admin-dashboard
```

```text
@.agents/skills/pos-admin-dashboard/SKILL.md build the users and roles module
```

Typical requests:

- "Set up the dashboard" on an empty folder scaffolds a Nuxt 4 project and builds the full admin shell.
- "Add a suppliers module" on an existing project adds the page, menu entry, permission keys, store, and demo service following the same conventions.
- "Check the POS screen on mobile" audits the layout rules and fixes anything that drifted.

### Stack

- Nuxt 4 (or Nuxt 3 with `compatibilityVersion: 4`), Vue 3, TypeScript
- Tailwind CSS (v3 or v4) with `@apply` in `main.css`
- Pinia for shared state
- `vue-multiselect` 3.x for every select control
- JSONC menu configuration
- Laravel as an optional, later API provider

### Customizing

The skill is a single Markdown file. Section 19 ("Editing This Skill") lists which section to edit for design tokens, menu and permission rules, POS layout rules, the users and roles module, and API contracts. Keep edits targeted so the verification and reporting rules stay intact.

## Contributing

Pull requests that add a new skill or tighten an existing one are welcome. Each skill lives in its own folder with a `SKILL.md` that starts with `name`, `description`, and `user-invocable` frontmatter.

## License

Released under the [MIT License](LICENSE).
