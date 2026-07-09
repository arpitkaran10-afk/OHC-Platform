# Incident Register — Module Context

> Part of the **TGHC OHC Platform v4.1**
> File: `OHC Screens/others_incident_register_v4/code.html`
> Nav path: Compliance & Audits › OSHWC Compliance › Incident Register

---

## Purpose

Records workplace incidents for legal compliance. The screen describes itself as "legally critical — all workplace incidents must be logged immediately." It combines an incident report entry form with a running history table.

---

## Layout

Two-column grid (8 + 4 on large screens, single column on small):

```
┌─────────────────────────────┬──────────────────┐
│  Page Header + Red Banner   │                  │
├─────────────────────────────┤                  │
│                             │  Incident        │
│  Incident Report Form       │  Visualizer      │
│  (left, 8-col)              │  (right, 4-col)  │
│                             │                  │
│                             │  Quick Stats     │
├─────────────────────────────┴──────────────────┤
│         Incident History Table (full width)    │
└────────────────────────────────────────────────┘
```

---

## Page Header

- **Title:** "Incident Register"
- **Subtitle:** "Legally critical — all workplace incidents must be logged immediately"
- **CTA button:** "Log New Incident" — red filled (`bg-[#EF4444]`), top-right

---

## Critical Banner

A red left-bordered alert bar below the header:

> "Incident reports are legally mandated. All fields marked required must be completed before submission."

Uses Tailwind classes: `bg-error-container text-on-error-container border-l-4 border-error`.

---

## Incident Report Form

Auto-generated ID shown in header: `INC-2023-0892 (Auto-Generated)`. Red left border (`border-[#EF4444]`) on card header.

### Fields

| Field | Type | Required | Notes |
|---|---|---|---|
| Visit Type | Static badge | — | Always "Incident" (red badge, `bg-error-container text-error`) |
| Date | `<input type="date">` | ✓ | Pre-filled: `2023-10-27` |
| Time of Incident | `<input type="time">` | ✓ | |
| Patient / Employee | Text search input | ✓ | Search by name or employee ID, has search icon |
| Building | `<select>` | ✓ | Options: Main Assembly Block, R&D Center, Logistics Warehouse |
| Work Related | Radio (Yes / No) | ✓ | |
| **Baseline Vitals** | Grouped 4-col sub-section | | Blue-labelled section header, `bg-surface-container-low` |
| › BP Systolic | Text input | ✓ | Placeholder: mmHg |
| › BP Diastolic | Text input | ✓ | Placeholder: mmHg |
| › Pulse | Text input | ✓ | Placeholder: bpm |
| › SpO2 | Text input | ✓ | Placeholder: % |
| Nature of Incident | Textarea (3 rows) | ✓ | |
| Root Cause | Textarea (2 rows) | — | Optional |
| First Aid / Care Given | Textarea (3 rows) | ✓ | |
| Ambulance Used | Checkbox | — | |
| Referred to Hospital | Radio (Yes / No) | ✓ | Triggers conditional fields when "Yes" |
| Hospital Name | Text input | — | Conditionally disabled when "Referred = No" |
| Accompanied By | `<select>` | — | Options: None, Staff Nurse, Paramedic, Co-worker |
| Escalation | Textarea (2 rows) | — | Optional |
| Follow-up Status | `<select>` | ✓ | Options: Stable, Under Observation, Hospitalised, Closed |
| Reported By Staff | `<select>` | ✓ | Options: Dr. Sarah Chen, Nurse Rajesh Kumar |
| Sign (Type Name) | Text input | ✓ | Italic serif font; labelled "Digital acknowledgement" |

### Form Actions
- **Submit Incident Report** — red filled button (`bg-[#EF4444]`)
- **Cancel** — outlined button

### Conditional Logic (JavaScript)
The "Referred to Hospital" radio controls the Hospital Name input:
- `referred = no` → disables input + adds `opacity-50` to its wrapper
- `referred = yes` → re-enables input

---

## Right Sidebar Widgets

### Incident Visualizer
- Square card with a centered stat badge: **12** Open Incidents (large red bold number)
- Background: `bg-surface-container`

### Quick Stats
Two progress-bar metrics:
| Metric | Value | Bar width |
|---|---|---|
| Last 30 Days | 08 | 65% (blue) |
| High Severity | 02 | 25% (red) |

---

## Incident History Table

Full-width card below the form + sidebar. Header: "Incident History" with two filter controls (Last 90 Days, Filters).

### Columns
| Column | Notes |
|---|---|
| Incident ID | Blue primary link-style text (e.g. `INC-2023-0889`) |
| Date | Formatted: `DD Mon YYYY` |
| Patient | Name + employee/contractor ID in parens |
| Building | Free text |
| Nature | Truncated with `max-w-[150px]` |
| Work Related | Badge: `YES` (blue `secondary-container`) |
| Status | Badge — see status badges below |
| Reported By | Staff short name |
| Actions | "View" text button (blue, underline on hover) |

### Status Badges
| Status | Style |
|---|---|
| CLOSED | `bg-surface-container-high text-on-surface-variant` (gray) |
| HOSPITALISED | `bg-error text-white` (red filled) |
| OBSERVATION | `bg-tertiary-container text-on-tertiary-container` (amber) |

### Row Highlight Rule
High-severity rows (e.g. Hospitalised) get a red left border and red-tinted row background:
```
class="bg-[#FEF2F2] hover:bg-[#FEE2E2] border-l-4 border-error"
```
Normal rows: `hover:bg-surface-bright transition-colors`

### Sample Data (3 rows)
| ID | Date | Patient | Building | Nature | Status |
|---|---|---|---|---|---|
| INC-2023-0889 | 24 Oct 2023 | John Doe (EMP-442) | Main Block | Laceration on right forearm | CLOSED |
| INC-2023-0885 | 22 Oct 2023 | Alice Smith (EMP-091) | Logistics | Fall from height, possible head injury | HOSPITALISED |
| INC-2023-0881 | 19 Oct 2023 | Sam Wilson (CONT-12) | R&D Center | Chemical splash in eye | OBSERVATION |

---

## Styling Notes

- This screen uses **both** `ohc-design-system.css` and **Tailwind CSS** (loaded via CDN with the `forms` and `container-queries` plugins). Most UI is Tailwind; the design system provides shell chrome only.
- Tailwind config extends the Material Design 3 colour palette (see full token list in `code.html`), with `primary: #0EA5E9` matching the OHC design system.
- Font sizes are defined as Tailwind custom tokens: `text-page-title` (20px/600), `text-section-heading` (16px/600), `text-label-sm` (11px/600), `text-body` (13px/400).
- The shell header and sidebar are present in the HTML but suppressed by the parent shell's injected `OVERRIDE_CSS` when loaded inside the app iframe.

---

## Known Issues / Observations

- The conditional "Accompanied By" select disable logic in JS uses `select:has(option:contains("None"))` — a non-standard CSS selector that will not work in all browsers; the Hospital Name disable works correctly but the Accompanied By select is not actually toggled.
- The sidebar nav in this file's own `<aside>` is a flat legacy list (not the accordion), reflecting an older code pattern before the shell took over navigation.
- The "Incident Visualizer" card is a placeholder — it shows a static number (12) with no actual chart or map rendering.
