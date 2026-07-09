# TGHC OHC Platform — Full Project Context

> **Version:** 4.1 · **Last updated:** 2026-07-03
> Feed this document to any AI assistant as context for working on this codebase.

---

## What Is This?

**TGHC OHC Platform** (TGH Clinic · Occupational Health Centre Platform) is a web-based clinical management system for occupational health centres (OHCs). It digitises the daily operational workflows: patient registration, pharmacy, biomedical waste tracking, compliance, analytics, wellness programmes, and finance.

- **Pure front-end** — HTML5, CSS3, and vanilla JavaScript only. No framework, no build step, no backend, no API.
- **Multi-site** — Trivandrum, Pune, Khordha. The sidebar has a site-selector dropdown (currently UI-only; no routing changes).
- **All data is hard-coded** — every screen is a high-fidelity UI prototype with static mock data.
- **Simulated user** — Kiran Prasad (initials KP), displayed in the shell header.
- **Deployment** — GitHub Pages at `arpitkaran10-afk/OHC-Platform`.
- **Git repo name** — `momentum-dashboard` (the repo was repurposed; ignore that name).

---

## Tech Stack

| Layer | Technology |
|---|---|
| Markup | HTML5 (self-contained per screen) |
| Styling | CSS3, custom properties, Flexbox, CSS Grid |
| Scripting | Vanilla JavaScript ES6+ (no frameworks) |
| Fonts | Google Fonts — Inter (body text) |
| Icons | Material Symbols Outlined (Google Fonts) |
| Build / Bundler | **None** — plain static files, served directly |
| Backend | **None** — all UI prototype / mockup |

---

## Canonical File Layout

The live application lives entirely inside `OHC Screens/`. Root-level module folders are older duplicates that predate the consolidation and should be ignored.

```
/TGHC OHC Platform/
├── index.html                          Redirect shim → OHC Screens/index.html
├── ohc-design-system.css               Root copy (legacy duplicate)
├── data.json                           UNRELATED — stock-screening data from prior project
├── PROJECT_CONTEXT.md                  ← this file
│
├── OHC Screens/                        ★ CANONICAL APPLICATION ROOT ★
│   ├── index.html                      App shell (header + sidebar + iframe host)
│   ├── ohc-design-system.css           Shared design system stylesheet
│   │
│   ├── ohc_dashboard_operational_control/code.html
│   ├── patients_opd_visit_entry/code.html
│   ├── patients_first_aid_register_itc/code.html
│   ├── patients_doctor_register_v4/code.html
│   ├── patients_ambulance_register/code.html
│   ├── patients_rest_case_allianz/code.html
│   ├── clinicians_staff_master_roster/code.html
│   ├── prescriptions_digital_rx_log/code.html
│   ├── pharmacy_stock_management_v4_1/code.html
│   ├── Oxygen Supply/code.html
│   ├── Vending Machine/code.html
│   ├── bmw_tracking_collection_log_v4/code.html
│   ├── health_camps_screening_events_v4/code.html
│   ├── Toolbox Training/code.html
│   ├── wellness_campaigns_calendar_v4/code.html
│   ├── compliance_audits_operational_hub_v4/code.html
│   ├── OSHWC Compliance/
│   │   ├── compliance-overview.html
│   │   ├── medical-surveillance.html
│   │   ├── incidents-diseases.html
│   │   ├── safety-audits.html
│   │   ├── hazardous-processes.html
│   │   └── hse-medical-dashboard-v2.html   (legacy, superseded)
│   ├── general_inventory_register_allianz/code.html
│   ├── analytics_reports_operational_insights_v4/code.html
│   ├── others_incident_register_v4/code.html
│   ├── invoice_tracker_allianz_finance_v4/code.html
│   ├── laundry_register_allianz_compliance_v4/code.html
│   └── settings_platform_configuration_v4/code.html
│
└── OSHWC Compliance/                   Root-level legacy copy — ignore
    └── hse-medical-dashboard-v2.html
```

Each module folder contains:
- `code.html` — the screen (always this filename, except the 5 OSHWC sub-screens)
- `screen.png` — reference screenshot used during development
- `DESIGN.md` — Material Design 3 colour-role spec (design reference only, not loaded at runtime)

---

## Architecture: Shell + Iframe Model

`OHC Screens/index.html` is the **permanent application shell**. It never unloads.

### Shell structure
```
┌──────────────────────────────────────────────── header (56px fixed) ──┐
│ TGHC logo │          global search          │ 🔔 Kiran Prasad  [KP]  │
├── sidebar (240px fixed) ──┬───────────────────────────────────────────┤
│  Site Selector dropdown   │                                           │
│  ─────────────────────    │         <iframe id="content-frame">       │
│  Accordion nav (NAV tree) │           module screen loads here        │
│  ─────────────────────    │                                           │
│  Help  v4.1               │                                           │
└───────────────────────────┴───────────────────────────────────────────┘
```

### Navigation engine (in `index.html`)
- `NAV` — a JavaScript array of leaf and group nodes (the single source of truth for all navigation).
- `LEAF_MAP` — flat `{ key → node }` map auto-derived from `NAV` at boot for O(1) lookups.
- `renderNav()` — recursively renders the `NAV` tree into the sidebar accordion DOM.
- `navigateTo(key)` — sets the iframe `src`, marks the active leaf, opens ancestor groups, updates the breadcrumb.
- `toggleGroup(key)` — opens/closes an accordion group without loading content.
- Loading spinner overlay (`#loading-overlay`) masks the iframe during transitions.

### Iframe isolation
- Each `code.html` is a fully self-contained HTML document with its own `<style>` and `<script>` blocks.
- After every iframe load, the shell injects an `OVERRIDE_CSS` `<style>` tag into the iframe document to **hide** the iframe's own `.ohc-header` and `.ohc-sidebar`, preventing double-rendered shell chrome.
- Iframe `sandbox` attribute: `allow-scripts allow-same-origin allow-forms allow-modals allow-popups`.

### Default screen
Dashboard loads automatically on page open (`navigateTo('dashboard')`).

---

## Navigation Tree (Current — v4.1)

> This is what the sidebar renders. Groups open/close but don't load content. Only leaves load the iframe.

```
Dashboard                               ohc_dashboard_operational_control/code.html

Clinic Management  [group]
  └─ Patients  [group]
       ├─ OPD Visits                    patients_opd_visit_entry/code.html
       ├─ First Aid Register            patients_first_aid_register_itc/code.html
       ├─ Doctor Register               patients_doctor_register_v4/code.html
       ├─ Ambulance Register            patients_ambulance_register/code.html
       └─ Rest Case                     patients_rest_case_allianz/code.html
  └─ Clinicians                         clinicians_staff_master_roster/code.html
  └─ Prescriptions                      prescriptions_digital_rx_log/code.html
  └─ Pharmacy  [group]
       ├─ Stock Management              pharmacy_stock_management_v4_1/code.html
       ├─ Oxygen Supply                 Oxygen Supply/code.html
       └─ Vending Machine              Vending Machine/code.html
  └─ Biomedical Waste                   bmw_tracking_collection_log_v4/code.html

Health Camps & Screenings               health_camps_screening_events_v4/code.html

Wellness  [group]
  ├─ Toolbox Trainings                  Toolbox Training/code.html
  └─ Other Wellness                     wellness_campaigns_calendar_v4/code.html

Compliance & Audits  [group]
  ├─ Compliance Matrix                  compliance_audits_operational_hub_v4/code.html
  └─ OSHWC Compliance  [group]
       ├─ Compliance Overview           OSHWC Compliance/compliance-overview.html
       ├─ Medical Surveillance          OSHWC Compliance/medical-surveillance.html
       ├─ Incidents & Diseases          OSHWC Compliance/incidents-diseases.html
       ├─ Safety Audits                 OSHWC Compliance/safety-audits.html
       ├─ Hazardous Processes           OSHWC Compliance/hazardous-processes.html
       └─ Incident Register             others_incident_register_v4/code.html
  └─ General Inventory                  general_inventory_register_allianz/code.html

Analytics                               analytics_reports_operational_insights_v4/code.html

Others  [group]
  ├─ Invoice Tracker                    invoice_tracker_allianz_finance_v4/code.html
  └─ Laundry Register                   laundry_register_allianz_compliance_v4/code.html

Settings                                settings_platform_configuration_v4/code.html
```

Total leaf screens: **25** (not counting the superseded `hse-medical-dashboard-v2.html`).

### Accordion behaviour
- Opening one group does **not** close others (multi-open).
- Navigating to a leaf automatically opens all its ancestor groups and marks them `.has-active`.
- Active leaf gets `.active` (primary-coloured left border + light blue background).
- Ancestor group headers get `.has-active` (bold text + subtle background).
- Breadcrumb updates to the full path, e.g. `OHC Platform › Compliance & Audits › OSHWC Compliance › Safety Audits`.

---

## Screen Inventory

| Screen | Folder / File | Purpose |
|---|---|---|
| Dashboard | `ohc_dashboard_operational_control/code.html` | Operational KPI overview — daily stats, alerts, quick-action tiles |
| OPD Visits | `patients_opd_visit_entry/code.html` | Outpatient visit registration form and log |
| First Aid Register | `patients_first_aid_register_itc/code.html` | First-aid case log and treatment record |
| Doctor Register | `patients_doctor_register_v4/code.html` | Doctor consultation log |
| Ambulance Register | `patients_ambulance_register/code.html` | Ambulance dispatch and transport log |
| Rest Case | `patients_rest_case_allianz/code.html` | Employees resting in the OHC register |
| Clinicians | `clinicians_staff_master_roster/code.html` | OHC clinician staff master roster |
| Prescriptions | `prescriptions_digital_rx_log/code.html` | Digital Rx / prescription log |
| Pharmacy — Stock | `pharmacy_stock_management_v4_1/code.html` | Medicine inventory and stock levels |
| Oxygen Supply | `Oxygen Supply/code.html` | Medical oxygen cylinder stock and usage |
| Vending Machine | `Vending Machine/code.html` | OHC vending machine dispensing log |
| Biomedical Waste | `bmw_tracking_collection_log_v4/code.html` | BMW collection log and tracking |
| Health Camps | `health_camps_screening_events_v4/code.html` | Health camp and screening event planning |
| Toolbox Training | `Toolbox Training/code.html` | Safety toolbox training session records |
| Wellness Campaigns | `wellness_campaigns_calendar_v4/code.html` | Wellness campaign calendar and management |
| Compliance Matrix | `compliance_audits_operational_hub_v4/code.html` | Compliance matrix and audit operational hub |
| OSHWC — Compliance Overview | `OSHWC Compliance/compliance-overview.html` | OSHWC regulatory compliance summary |
| OSHWC — Medical Surveillance | `OSHWC Compliance/medical-surveillance.html` | Periodic medical examination tracking |
| OSHWC — Incidents & Diseases | `OSHWC Compliance/incidents-diseases.html` | Occupational incidents and disease reporting |
| OSHWC — Safety Audits | `OSHWC Compliance/safety-audits.html` | Safety audit schedule and findings |
| OSHWC — Hazardous Processes | `OSHWC Compliance/hazardous-processes.html` | Hazardous process exposure register |
| Incident Register | `others_incident_register_v4/code.html` | General incident log |
| General Inventory | `general_inventory_register_allianz/code.html` | Consumables and equipment inventory |
| Analytics | `analytics_reports_operational_insights_v4/code.html` | Operational insights, charts, downloadable reports |
| Invoice Tracker | `invoice_tracker_allianz_finance_v4/code.html` | Finance invoice tracking (Allianz billing) |
| Laundry Register | `laundry_register_allianz_compliance_v4/code.html` | Linen/laundry compliance register |
| Settings | `settings_platform_configuration_v4/code.html` | Platform config — users, site settings, preferences |

---

## Design System (`ohc-design-system.css`)

**Path:** `OHC Screens/ohc-design-system.css`
All module screens must link this file: `<link rel="stylesheet" href="../ohc-design-system.css">` (adjust relative path if nested).

### CSS Custom Property Tokens

| Token | Value | Role |
|---|---|---|
| `--ohc-primary` | `#0EA5E9` | Primary actions, active nav, links |
| `--ohc-primary-dark` | `#0369A1` | Hover state on primary |
| `--ohc-primary-light` | `#E0F2FE` | Focus rings, light highlights |
| `--ohc-success` | `#10B981` | Positive / compliant / OK |
| `--ohc-warning` | `#F59E0B` | Caution / pending |
| `--ohc-error` | `#EF4444` | Danger / overdue / critical |
| `--ohc-n-50` | `#F8FAFC` | Lightest neutral (page bg) |
| `--ohc-n-100` | `#F1F5F9` | Card bg, zebra stripe |
| `--ohc-n-200` | `#E2E8F0` | Borders, dividers |
| `--ohc-n-300` | `#CBD5E1` | Disabled / placeholder |
| `--ohc-n-400` | `#94A3B8` | Secondary text |
| `--ohc-n-500` | `#64748B` | Muted text |
| `--ohc-n-600` | `#475569` | Body text |
| `--ohc-n-700` | `#334155` | Emphasis text |
| `--ohc-n-800` | `#1E293B` | Headings |
| `--ohc-n-900` | `#0F172A` | Max contrast |
| `--ohc-header-h` | `56px` | Fixed header height |
| `--ohc-sidebar-w` | `240px` | Fixed sidebar width |

### Component Classes

| Class | Description |
|---|---|
| `.ohc-card` | White card with border-radius and subtle shadow |
| `.ohc-kpi-card` | KPI stat tile — icon, number, label, trend indicator |
| `.ohc-chip-active` | Green pill — "Active" / "Compliant" |
| `.ohc-chip-pending` | Yellow pill — "Pending" |
| `.ohc-chip-inactive` | Gray pill — "Inactive" |
| `.ohc-chip-na` | Light gray pill — "N/A" |
| `.ohc-chip-new` | Blue pill — "New" |
| `.ohc-btn-primary` | Filled blue button |
| `.ohc-btn-secondary` | Outlined button |
| `.ohc-btn-danger` | Filled red button |
| `.ohc-icon-btn` | Icon-only button (circle hover) |
| `.ohc-table` | Full-width data table with hover rows |
| `.ohc-filter-bar` | Horizontal row of search + filter inputs |
| `.ohc-input` | Text input |
| `.ohc-select` | Select dropdown |
| `.ohc-textarea` | Textarea |
| `.ohc-modal` | Modal dialog box |
| `.ohc-overlay` | Semi-transparent dark backdrop |
| `.ohc-alert-info` | Blue informational banner |
| `.ohc-alert-success` | Green success banner |
| `.ohc-alert-warning` | Yellow warning banner |
| `.ohc-alert-critical` | Red critical banner |
| `.ohc-empty-state` | Centered empty-state illustration + message |
| `.ohc-tab-bar` | Horizontal tab row container |
| `.ohc-tab` | Individual tab; `.ohc-tab.active` = selected |
| `.ohc-page-header` | Page title + subtitle + action buttons row |

### Shell-only CSS (defined in `index.html`, not the design system)

| Class | Purpose |
|---|---|
| `.nav-row` | Base sidebar row (both leaf `<a>` and group `<button>`) |
| `.nav-row--l1 / l2 / l3` | Indent levels (l1=top, l2=children, l3=grandchildren) |
| `.nav-icon` | Material icon within a nav row |
| `.nav-label` | Flex-grow text label |
| `.nav-chevron` | Chevron icon on groups; rotates 180° when `.open` |
| `.nav-group` | Wrapper for group header + collapsible body |
| `.nav-group.open` | Expanded — triggers `grid-template-rows: 1fr` |
| `.nav-group-body` | CSS Grid height-transition container (`0fr → 1fr`) |
| `.nav-group-body-inner` | `overflow: hidden` wrapper required for grid collapse |
| `.nav-row.active` | Active leaf: primary left border + light blue bg |
| `.nav-row.has-active` | Group header with active descendant: bold + subtle bg |

---

## UI Conventions

- **Header** (56px fixed): Logo "TGHC" left | Global search center | Bell + "Kiran Prasad" + [KP] avatar right.
- **Sidebar** (240px fixed): Site-selector dropdown at top → accordion nav → Help link + "v4.1" footer.
- **Content area**: Begins at `top: 56px; left: 240px`. The iframe fills all remaining space.
- **No horizontal tab bar** — the `#tab-bar-wrapper` / `.shell-tab` bar was removed in v4.1. Sub-module navigation is exclusively in the sidebar accordion. Do not re-add a tab bar.
- **Icons**: Material Symbols Outlined (rounded variant) loaded from Google Fonts.
- **Colour rule**: Never hardcode hex values. Always use `--ohc-*` design tokens.
- **Font**: Inter via Google Fonts; system-ui fallback. `-webkit-font-smoothing: antialiased`.

---

## How to Add a New Screen

1. Create `OHC Screens/<module_folder>/code.html`.
2. In `code.html`, link the design system: `<link rel="stylesheet" href="../ohc-design-system.css">`.
3. Include a `<div class="ohc-header">` and `<div class="ohc-sidebar">` block (the shell will hide them via injected CSS, but they're needed for standalone viewing).
4. Add a leaf node to the `NAV` array in `OHC Screens/index.html`:
   ```js
   { key: 'my_screen', label: 'My Screen', icon: 'icon_name', file: 'my_screen_folder/code.html' }
   ```
   If nesting under a group, add it to that group's `children` array.
5. `LEAF_MAP` is rebuilt automatically from `NAV` — no other registration needed.

### Folder naming convention
- New folders: `snake_case_descriptive` (e.g. `patient_referral_tracker`).
- Legacy exceptions with Title Case + spaces: `Oxygen Supply/`, `Toolbox Training/`, `Vending Machine/`, `OSHWC Compliance/`.
- Screen HTML file is always `code.html` (exception: OSHWC sub-screens use descriptive names like `compliance-overview.html`).

---

## Key Constraints

1. **No framework, no build step** — every screen is self-contained HTML. Do not introduce npm, Vite, React, or any bundler.
2. **Iframe isolation** — modules run in their own document. Cross-module communication must go through the parent shell.
3. **Design system first** — import `ohc-design-system.css` and use `--ohc-*` tokens. No hardcoded colours.
4. **No tab bar** — removed in v4.1. Do not re-add.
5. **data.json is irrelevant** — both `data.json` files contain stock-screening data from the original `momentum-dashboard` repo. No OHC screen references them.
6. **OSHWC legacy file** — `OSHWC Compliance/hse-medical-dashboard-v2.html` is superseded by the five individual sub-screens. Do not link to it from the nav.
7. **ohc_operational_control/** — this folder contains only a `DESIGN.md`; there is no `code.html`. It is a planned screen that has not been built yet.

---

## Files to Ignore

| File / Folder | Reason |
|---|---|
| `data.json` (root + `OHC Screens/`) | Stock-screening data from a prior project; unrelated to OHC |
| `README.md` | Placeholder: contains only `# momentum-dashboard` |
| `OSHWC Compliance/hse-medical-dashboard-v2.html` | Superseded by five OSHWC sub-screens |
| All root-level module folders (e.g. `patients_opd_visit_entry/`) | Older duplicates predating `OHC Screens/` consolidation |
| `ohc_operational_control/DESIGN.md` | Design spec only; screen not yet built |
