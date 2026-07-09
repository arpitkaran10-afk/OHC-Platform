# TGHC OHC Platform — Full Project Context

> **Version:** 4.2 · **Last updated:** 2026-07-09
> Feed this document to any AI assistant as context for working on this codebase.

---

## What Is This?

**TGHC OHC Platform** (TGH Clinic · Occupational Health Centre Platform) is a web-based clinical management system for occupational health centres (OHCs). It digitises the daily operational workflows: patient registration, pharmacy, biomedical waste tracking, compliance, analytics, wellness programmes, and finance.

- **Pure front-end** — HTML5, CSS3, and vanilla JavaScript only. No framework, no build step, no backend, no API.
- **Multi-site** — Trivandrum, Pune, Khordha. The sidebar has a site-selector dropdown (currently UI-only; no routing changes).
- **All data is hard-coded** — every screen is a high-fidelity UI prototype with static mock data.
- **Simulated user** — Kiran Prasad (initials KP), displayed in the shell header.
- **Deployment** — GitHub Pages at `arpitkaran10-afk/OHC-Platform` → https://arpitkaran10-afk.github.io/OHC-Platform/OHC%20Screens/index.html
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

> **Note on Tailwind:** Several older screens (OPD Visits, First Aid Register, Doctor Register, Ambulance Register, Rest Case, Pharmacy Stock, etc.) still load Tailwind CSS from CDN (`https://cdn.tailwindcss.com`) for their existing markup. **Do not add Tailwind to new screens** — use `ohc-design-system.css` tokens and plain CSS only. Never remove Tailwind from files that already use it (that would break their existing layout). New additions to existing Tailwind files can also use Tailwind classes for consistency within that file.

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

### Navigating between modules from within an iframe
Call the parent shell's `navigateTo(key)` from inside any module screen:
```js
window.parent.navigateTo('patients_opd');
```
This is how dashboard stat cards navigate to their respective modules on click.

### Default screen
Dashboard loads automatically on page open (`navigateTo('dashboard')`).

### Cache-busting in NAV
Several NAV `file` entries include `?v=N` query strings (e.g. `code.html?v=4`). These are cache-bust tokens — bump the number whenever you edit a file so the browser fetches the new version. **Do not remove these version parameters.**

---

## Navigation Tree (Current — v4.2)

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

Total leaf screens: **25**.

---

## Screen Inventory

| Screen | Folder / File | Purpose | Notes |
|---|---|---|---|
| Dashboard | `ohc_dashboard_operational_control/code.html` | Operational KPI overview — clickable stat cards, charts, wired to modules | All cards wired via `window.parent.navigateTo()` |
| OPD Visits | `patients_opd_visit_entry/code.html` | **List-first**: patient visit log table (default) + form behind "+ New Visit" | 20 demo rows, filter pills (Today/Week/Month/Quarter/Year) |
| First Aid Register | `patients_first_aid_register_itc/code.html` | **List-first**: box check log table (default) + form behind "+ New Entry" | Stats widgets shown in list view |
| Doctor Register | `patients_doctor_register_v4/code.html` | **List-first**: consultation history table (default) + form behind "+ New Consultation" | History table built fresh; form has 5 cards |
| Ambulance Register | `patients_ambulance_register/code.html` | **List-first**: trip log table (default) + form behind "+ Log New Trip" | Filter bar + table toggled with 3-div pattern |
| Rest Case | `patients_rest_case_allianz/code.html` | **List-first**: Rest Case History table (default) + form behind "+ New Rest Entry" | Ailment Tally also behind the form |
| Clinicians | `clinicians_staff_master_roster/code.html` | OHC clinician staff master roster | |
| Prescriptions | `prescriptions_digital_rx_log/code.html` | Digital Rx / prescription log | |
| Pharmacy — Stock | `pharmacy_stock_management_v4_1/code.html` | Medicine inventory and stock levels | 11-medicine list |
| Oxygen Supply | `Oxygen Supply/code.html` | Medical oxygen cylinder stock and usage | |
| Vending Machine | `Vending Machine/code.html` | OHC vending machine dispensing log | |
| Biomedical Waste | `bmw_tracking_collection_log_v4/code.html` | BMW collection log and tracking | |
| Health Camps | `health_camps_screening_events_v4/code.html` | Health camp and screening event planning | |
| Toolbox Training | `Toolbox Training/code.html` | Safety toolbox training session records | |
| Wellness Campaigns | `wellness_campaigns_calendar_v4/code.html` | Wellness campaign calendar and management | |
| Compliance Matrix | `compliance_audits_operational_hub_v4/code.html` | Compliance matrix and audit hub | Has Compliance Overview + Audit Overview widgets |
| OSHWC — Compliance Overview | `OSHWC Compliance/compliance-overview.html` | OSHWC regulatory compliance summary | |
| OSHWC — Medical Surveillance | `OSHWC Compliance/medical-surveillance.html` | Periodic medical examination tracking | Tab-based; large clinical form |
| OSHWC — Incidents & Diseases | `OSHWC Compliance/incidents-diseases.html` | Occupational incidents and disease reporting | |
| OSHWC — Safety Audits | `OSHWC Compliance/safety-audits.html` | Safety audit schedule and findings | Already list-first, no entry form |
| OSHWC — Hazardous Processes | `OSHWC Compliance/hazardous-processes.html` | Hazardous process exposure register | |
| Incident Register | `others_incident_register_v4/code.html` | Full 9-section incident & investigation report (A–I) | Major redesign; sticky sidebar (sections A+B only), sections C–I full-width |
| General Inventory | `general_inventory_register_allianz/code.html` | Consumables and equipment inventory | Already table-first with sidebar form |
| Analytics | `analytics_reports_operational_insights_v4/code.html` | Operational insights, charts, reports | Medicine trend scrollable list, heatmap with hour-blocks |
| Invoice Tracker | `invoice_tracker_allianz_finance_v4/code.html` | Finance invoice tracking (Allianz billing) | |
| Laundry Register | `laundry_register_allianz_compliance_v4/code.html` | Linen/laundry compliance register | |
| Settings | `settings_platform_configuration_v4/code.html` | Platform config — users, site settings, preferences | |

---

## The "List-First" Pattern (applied to all Patients registers)

All register-style screens under Patients follow this UX pattern:

1. **Default view = the history/log table** — full-width, visible immediately on load.
2. **"+ New Entry" button** (top-right) swaps the table out and shows the data-entry form.
3. **Cancel / Save buttons** in the form return to the list view.
4. Implemented using **three sibling `<div>` elements** — not nesting:
   ```html
   <div id="view-list">     <!-- page header + CTA button -->
   <div id="view-form">     <!-- the entry form (display:none by default) -->
   <div id="view-list-table"> <!-- filter bar + table -->
   ```
   JS toggles all three:
   ```js
   function showForm() {
     document.getElementById('view-list').style.display = 'none';
     document.getElementById('view-list-table').style.display = 'none';
     document.getElementById('view-form').style.display = 'block';
   }
   function showList() {
     document.getElementById('view-form').style.display = 'none';
     document.getElementById('view-list').style.display = 'block';
     document.getElementById('view-list-table').style.display = 'block';
   }
   ```
   > **Critical**: Do NOT nest `view-form` inside `view-list`. They must be siblings. Nesting causes the form to disappear when the list header is hidden.

---

## Dashboard Wiring

Every stat card and chart widget on the Dashboard is clickable via `data-nav` attributes. The shell's JS reads these and calls `window.parent.navigateTo(key)`:

| Dashboard Widget | `data-nav` key | Destination |
|---|---|---|
| Consultations Today | `patients_opd` | Patients > OPD Visits |
| Rest Cases Today | `patients_rest` | Patients > Rest Case |
| Active Clinicians | `clinicians` | Clinicians |
| Low Stock Alerts | `pharmacy_stock` | Pharmacy > Stock Management |
| BMW Pickup Pending | `bmw` | Biomedical Waste |
| Incidents This Month | `oshwc_incident_register` | Incident Register |
| Training Sessions | `toolbox` | Wellness > Toolbox Trainings |
| Pending Follow-ups | `compliance_matrix` | Compliance Matrix |
| Footfall by Staff Type | `analytics` | Analytics |
| OPD vs Rest Split | `patients_opd` | Patients > OPD Visits |
| Top 5 Ailments | `analytics` | Analytics |
| Frequent Visitor Alerts | `patients_opd` | Patients > OPD Visits |
| Medicine Stock Alerts | `pharmacy_stock` | Pharmacy > Stock Management |
| Medicine Consumption Trend | `pharmacy_stock` | Pharmacy > Stock Management |
| MoM Footfall Trend | `analytics` | Analytics |
| Staff Attendance — Today | `clinicians` | Clinicians |
| MoM Action Items — "BMW Audit Remediation" row | inline `onclick` → `bmw` | Biomedical Waste |
| MoM Action Items — "Ergonomics Workshop Q4" row | inline `onclick` → `toolbox` | Toolbox Trainings |

---

## Incident Register — Key Implementation Details

File: `others_incident_register_v4/code.html`

- **9 lettered sections (A–I)** with sticky section-jump nav chips at top.
- **Layout split**: Sections A + B share a two-column grid with the sidebar (Open Incidents, Quick Stats, Report Progress). Sections C–I and all subsequent content (sign-off, history table) are in a **separate full-width div** below that grid — this avoids the dead-space problem where the sidebar column reserves width all the way to the bottom.
- **No Tailwind** — pure CSS in `<style>` block + `ohc-design-system.css`.
- **Sections E and F** are collapsible accordions with "N selected" count badges.
- **Sections G/H/I** are rendered as a phase workflow (arrows connecting three cards).
- **Incident ID format**: `DDMMYYYY/N` (client-facing, shown in Section A next to the Facility/Site label). Internal reference `INC-YYYY-NNNN` also stored but not shown in the header.
- **History table** at the bottom has columns: Incident ID, Date, Patient, Building, Nature, Incident Type, Work Related, Severity, OHS Recordable, Status, Reported By.

---

## Analytics Page — Key Implementation Details

File: `analytics_reports_operational_insights_v4/code.html`

- **Medicine Consumption Trend**: Full 11-medicine scrollable list (`max-height:480px; overflow-y:auto`). Script must come **after** the `<div id="med-trend-rows">` container in HTML (script-before-container causes silent failure with empty widget).
- **Footfall by Staff Type**: Side-by-side grouped bar chart — two adjacent flex bars per building (Direct Employees + Contract Staff). NOT using absolute-positioned overlapping bars.
- **Shift-wise Coverage Heatmap**: JS-generated hourly sub-blocks (8 slim bars per shift×day cell). Hover tooltips show exact hour range + coverage %.
- **Total OHC Footfall / Cut Wound Deep Dive**: SVG donuts (not CSS border-trick).

---

## Pharmacy Stock Management — Medicine List

File: `pharmacy_stock_management_v4_1/code.html`

Current 11-medicine inventory (used consistently in both Stock Management and Analytics > Medicine Consumption Trend):

| Medicine | Unit |
|---|---|
| Paracetamol 500mg | tabs |
| Ibuprofen 400mg | tabs |
| Cetirizine 10mg | tabs |
| Amoxicillin 500mg | caps |
| Metformin 850mg | tabs |
| Azithromycin 500mg | tabs |
| Omeprazole 20mg | caps |
| Antacids (Gelusil) | ml |
| ORS Sachets | units |
| Diclofenac Gel 1% | tubes |
| Chloramphenicol Eye Drops | vials |

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

---

## UI Conventions

- **Header** (56px fixed): Logo "TGHC" left | Global search center | Bell + "Kiran Prasad" + [KP] avatar right.
- **Sidebar** (240px fixed): Site-selector dropdown at top → accordion nav → Help link + "v4.1" footer.
- **Content area**: Begins at `top: 56px; left: 240px`. The iframe fills all remaining space.
- **No horizontal tab bar** — the `#tab-bar-wrapper` / `.shell-tab` bar was removed in v4.1. Sub-module navigation is exclusively in the sidebar accordion. Do not re-add a tab bar.
- **Icons**: Material Symbols Outlined (rounded variant) loaded from Google Fonts.
- **Colour rule**: Never hardcode hex values. Always use `--ohc-*` design tokens (or Tailwind semantic tokens in files that already use Tailwind).
- **Font**: Inter via Google Fonts; system-ui fallback. `-webkit-font-smoothing: antialiased`.
- **No horizontal scroll** — all pages use `html, body { overflow-x: hidden }` and `table-layout: fixed` with `colgroup` percentage widths for tables. Never introduce overflow-x on any screen.

---

## How to Add a New Screen

1. Create `OHC Screens/<module_folder>/code.html`.
2. Link the design system: `<link rel="stylesheet" href="../ohc-design-system.css">`.
3. Include `.ohc-header` and `.ohc-sidebar` markup (shell hides them at runtime; needed for standalone viewing). Add this to the screen's own `<style>` to prevent the flash of wrong margins:
   ```css
   .ohc-main  { margin-left: 0 !important; padding-top: 0 !important; }
   .ohc-header, .ohc-sidebar { display: none !important; }
   ```
4. Add a leaf node to the `NAV` array in `OHC Screens/index.html`.
5. If the screen is a register/log, follow the **List-First Pattern** (see above).
6. Bump the `?v=N` cache-bust parameter on the NAV entry whenever you modify the file.

---

## Key Constraints

1. **No framework, no build step** — every screen is self-contained HTML. Do not introduce npm, Vite, React, or any bundler.
2. **Iframe isolation** — modules run in their own document. Cross-module communication must go through the parent shell via `window.parent.navigateTo(key)`.
3. **Design system first** — import `ohc-design-system.css` and use `--ohc-*` tokens. No hardcoded colours.
4. **No tab bar** — removed in v4.1. Do not re-add.
5. **data.json is irrelevant** — both `data.json` files contain stock-screening data from the original `momentum-dashboard` repo. No OHC screen references them.
6. **OSHWC legacy file** — `OSHWC Compliance/hse-medical-dashboard-v2.html` is superseded by the five individual sub-screens. Do not link to it from the nav.
7. **ohc_operational_control/** — contains only a `DESIGN.md`; no `code.html`. Planned screen, not yet built.
8. **Tailwind CDN** — several existing screens load Tailwind. Never remove it from files that use it, and never add it to new screens. New screens use `ohc-design-system.css` only.
9. **Script-before-container bug** — if a `<script>` block uses `getElementById()` to populate a container, the container `<div>` must appear **before** the `<script>` in the HTML. Script executing before its target exists returns `null` and silently renders nothing.

---

## Files to Ignore

| File / Folder | Reason |
|---|---|
| `data.json` (root + `OHC Screens/`) | Stock-screening data from a prior project; unrelated to OHC |
| `README.md` | Placeholder: contains only `# momentum-dashboard` |
| `OSHWC Compliance/hse-medical-dashboard-v2.html` | Superseded by five OSHWC sub-screens |
| All root-level module folders (e.g. `patients_opd_visit_entry/`) | Older duplicates predating `OHC Screens/` consolidation |
| `ohc_operational_control/DESIGN.md` | Design spec only; screen not yet built |
