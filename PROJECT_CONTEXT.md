# TGHC OHC Platform — Project Context

## What Is This?
**TGHC OHC Platform** (TGH Clinic · Occupational Health Centre Platform) is a web-based clinical management system for occupational health centres. It is a pure front-end application — HTML, CSS, and vanilla JavaScript only — with no backend framework or build toolchain. All screens are static HTML files that simulate a multi-module SPA.

The platform is currently at **v4.0** and is used across multiple sites (Trivandrum, Pune, Khordha).

---

## Architecture

### Shell + Iframe Model
- `index.html` is the **permanent shell**: fixed header, fixed sidebar, and a central `<iframe id="content-frame">` that swaps in module screens.
- A JavaScript `NAV` tree drives navigation: each node is either a **leaf** (has `file`) or a **group** (has `children`). A flat `LEAF_MAP` is derived from the tree for O(1) lookups.
- The sidebar renders a collapsible accordion from the `NAV` tree. There is **no horizontal tab bar** — all sub-navigation lives in the sidebar.
- A loading spinner overlay (`#loading-overlay`) hides the iframe during transitions.
- After each iframe load, the shell injects a small override `<style>` block into the iframe document that hides the iframe page's own `.ohc-header` and `.ohc-sidebar` (so module screens don't double-render shell chrome).

### File Layout
```
OHC Screens/
├── index.html                          ← Shell (header + sidebar + iframe)
├── ohc-design-system.css               ← Shared design tokens & component styles
├── <module_folder>/
│   ├── code.html                       ← The module's actual screen
│   └── screen.png                      ← Reference screenshot
```

---

## Design System (`ohc-design-system.css`)

All module screens link this shared stylesheet. It defines:

### CSS Custom Properties (Design Tokens)
| Token | Value | Use |
|---|---|---|
| `--ohc-primary` | `#0EA5E9` | Primary actions, active states |
| `--ohc-primary-dark` | `#0369A1` | Hover states |
| `--ohc-primary-light` | `#E0F2FE` | Light highlights |
| `--ohc-success` | `#10B981` | Positive / OK |
| `--ohc-warning` | `#F59E0B` | Caution |
| `--ohc-error` | `#EF4444` | Danger / critical |
| `--ohc-n-50..900` | Grays | Neutral scale |
| `--ohc-header-h` | `56px` | Fixed header height |
| `--ohc-sidebar-w` | `240px` | Fixed sidebar width |

### Typography
- Font: `Inter` (Google Fonts), system-ui fallback
- Smooth rendering via `-webkit-font-smoothing: antialiased`

### Shell Components
- `.ohc-header` — fixed top bar with logo, search, notification bell, user avatar
- `.ohc-sidebar` — fixed left nav with site selector, accordion nav, footer
- `.ohc-logo` — brand text "TGHC" in primary blue
- `.ohc-avatar` — circular initials badge

### Shell-only CSS classes (defined in `index.html`, not the design system)
| Class | Purpose |
|---|---|
| `.nav-row` | Base style for every sidebar row (both leaf `<a>` and group `<button>`) |
| `.nav-row--l1/2/3` | Indent levels: l1 = top, l2 = children, l3 = grandchildren |
| `.nav-icon` | Material icon within a nav row |
| `.nav-label` | Flex-grow text label |
| `.nav-chevron` | Chevron icon on group rows; rotates 180° when `.open` |
| `.nav-group` | Wrapper `<div>` for accordion group + its body |
| `.nav-group.open` | Expanded state — triggers `grid-template-rows: 1fr` on body |
| `.nav-group-body` | CSS Grid height-transition container (`0fr → 1fr`) |
| `.nav-group-body-inner` | `overflow: hidden` inner wrapper required for grid collapse |
| `.nav-row.active` | Active leaf: primary left border + light blue bg |
| `.nav-row.has-active` | Group header with an active descendant: bold + subtle bg |

---

## Navigation Structure

The sidebar uses a **two-level accordion**. Groups do not load content themselves — only leaf items do.

```
Dashboard                           → ohc_dashboard_operational_control/code.html

Clinic Management  [group]
  └─ Patients  [group]
       ├─ OPD Visits                → patients_opd_visit_entry/code.html
       ├─ First Aid Register        → patients_first_aid_register_itc/code.html
       ├─ Doctor Register           → patients_doctor_register_v4/code.html
       ├─ Ambulance Register        → patients_ambulance_register/code.html
       └─ Rest Case                 → patients_rest_case_allianz/code.html
  └─ Clinicians                     → clinicians_staff_master_roster/code.html
  └─ Prescriptions                  → prescriptions_digital_rx_log/code.html
  └─ Pharmacy  [group]
       ├─ Stock Management          → pharmacy_stock_management_v4_1/code.html
       ├─ Oxygen Supply             → Oxygen Supply/code.html
       └─ Vending Machine           → Vending Machine/code.html
  └─ Biomedical Waste               → bmw_tracking_collection_log_v4/code.html

Health Camps & Screenings           → health_camps_screening_events_v4/code.html

Wellness  [group]
  ├─ Toolbox Trainings              → Toolbox Training/code.html
  └─ Other Wellness                 → wellness_campaigns_calendar_v4/code.html

Compliance & Audits  [group]
  ├─ Compliance Matrix              → compliance_audits_operational_hub_v4/code.html
  ├─ OSHWC Compliance               → OSHWC Compliance/hse-medical-dashboard-v2.html
  └─ General Inventory              → general_inventory_register_allianz/code.html

Analytics                           → analytics_reports_operational_insights_v4/code.html

Others  [group]
  ├─ Incident Register              → others_incident_register_v4/code.html
  ├─ Invoice Tracker                → invoice_tracker_allianz_finance_v4/code.html
  └─ Laundry Register               → laundry_register_allianz_compliance_v4/code.html

Settings                            → settings_platform_configuration_v4/code.html
```

### Accordion Behavior
- **Groups**: clicking toggles open/closed, does NOT load anything into the iframe.
- **Leaf items**: clicking loads `<folder>/code.html` into the iframe.
- **Multi-open**: opening one group does not close others.
- **Auto-expand**: navigating to a leaf automatically opens all its ancestor groups and marks them `.has-active`.
- **Active state**: active leaf gets `.active` (primary left border + bg); ancestor group headers get `.has-active` (bold).
- **Breadcrumb**: updates to the full path, e.g. `OHC Platform › Clinic Management › Patients › OPD Visits`.
- **Default load**: Dashboard on page load.

---

## All Screen Folders
```
ohc_dashboard_operational_control/
patients_opd_visit_entry/
patients_first_aid_register_itc/
patients_doctor_register_v4/
patients_ambulance_register/
patients_rest_case_allianz/
clinicians_staff_master_roster/
prescriptions_digital_rx_log/
pharmacy_stock_management_v4_1/
Oxygen Supply/
Vending Machine/
health_camps_screening_events_v4/
Toolbox Training/
wellness_campaigns_calendar_v4/
compliance_audits_operational_hub_v4/
OSHWC Compliance/
general_inventory_register_allianz/
laundry_register_allianz_compliance_v4/
bmw_tracking_collection_log_v4/
analytics_reports_operational_insights_v4/
invoice_tracker_allianz_finance_v4/
others_incident_register_v4/
ohc_operational_control/
settings_platform_configuration_v4/
```

---

## UI Conventions

- **Header** (56px fixed): Logo "TGHC" (left) | Global search input (center) | Notifications bell + user name + avatar (right). Current user: *Kiran Prasad (KP)*.
- **Sidebar** (240px fixed): Site selector dropdown at top, accordion nav, help + version footer.
- **Content area**: Starts at `top: 56px; left: 240px`. The iframe fills the entire remaining space — there is no tab bar above it.
- **Icons**: Material Symbols Outlined from Google Fonts.
- **Iframe sandbox**: `allow-scripts allow-same-origin allow-forms allow-modals allow-popups`.

---

## Tech Stack Summary
| Layer | Technology |
|---|---|
| Markup | HTML5 |
| Styling | CSS3, custom properties, flexbox/grid (CSS Grid used for accordion animation) |
| Scripting | Vanilla JavaScript (ES6+) |
| Fonts/Icons | Google Fonts (Inter), Material Symbols Outlined |
| Build/Bundler | None — plain static files |
| Backend | None — all UI prototype / mockup |

---

## Key Constraints & Patterns
1. **No framework** — every screen is self-contained HTML. Shared state lives only in the shell's JS.
2. **Iframe isolation** — each module screen runs independently; cross-module communication goes through the parent shell if needed.
3. **Design system first** — all new screens must import `ohc-design-system.css` and use `--ohc-*` tokens. Do not hardcode colours.
4. **No horizontal tab bar** — sub-module navigation belongs in the sidebar accordion, not above the iframe.
5. **Naming convention** — folders use snake_case, descriptive names (`module_feature_version`). Screen HTML file is always `code.html`. Exception: some older folders use Title Case with spaces (e.g. `Oxygen Supply/`, `Toolbox Training/`).
6. **Sites** — the platform is multi-site; the sidebar site selector is a global context switcher (currently UI-only).
7. **NAV tree shape** — when adding a new screen, add a leaf node (`{ key, label, icon, file }`) to the `NAV` array in `index.html`. If it belongs to an existing group, nest it under that group's `children`. `LEAF_MAP` is built automatically.
