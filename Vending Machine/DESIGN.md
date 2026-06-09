---
name: Clinical Intelligence Core
colors:
  surface: '#f9f9ff'
  surface-dim: '#d3daef'
  surface-bright: '#f9f9ff'
  surface-container-lowest: '#ffffff'
  surface-container-low: '#f1f3ff'
  surface-container: '#e9edff'
  surface-container-high: '#e1e8fd'
  surface-container-highest: '#dce2f7'
  on-surface: '#141b2b'
  on-surface-variant: '#3e4850'
  inverse-surface: '#293040'
  inverse-on-surface: '#edf0ff'
  outline: '#6e7881'
  outline-variant: '#bec8d2'
  surface-tint: '#006591'
  primary: '#006591'
  on-primary: '#ffffff'
  primary-container: '#0ea5e9'
  on-primary-container: '#003751'
  inverse-primary: '#89ceff'
  secondary: '#006c49'
  on-secondary: '#ffffff'
  secondary-container: '#6cf8bb'
  on-secondary-container: '#00714d'
  tertiary: '#855300'
  on-tertiary: '#ffffff'
  tertiary-container: '#d88a00'
  on-tertiary-container: '#4a2c00'
  error: '#ba1a1a'
  on-error: '#ffffff'
  error-container: '#ffdad6'
  on-error-container: '#93000a'
  primary-fixed: '#c9e6ff'
  primary-fixed-dim: '#89ceff'
  on-primary-fixed: '#001e2f'
  on-primary-fixed-variant: '#004c6e'
  secondary-fixed: '#6ffbbe'
  secondary-fixed-dim: '#4edea3'
  on-secondary-fixed: '#002113'
  on-secondary-fixed-variant: '#005236'
  tertiary-fixed: '#ffddb8'
  tertiary-fixed-dim: '#ffb95f'
  on-tertiary-fixed: '#2a1700'
  on-tertiary-fixed-variant: '#653e00'
  background: '#f9f9ff'
  on-background: '#141b2b'
  surface-variant: '#dce2f7'
typography:
  page-title:
    fontFamily: Inter
    fontSize: 20px
    fontWeight: '600'
    lineHeight: 28px
    letterSpacing: -0.01em
  section-heading:
    fontFamily: Inter
    fontSize: 16px
    fontWeight: '600'
    lineHeight: 24px
  body:
    fontFamily: Inter
    fontSize: 13px
    fontWeight: '400'
    lineHeight: 20px
  label:
    fontFamily: Inter
    fontSize: 13px
    fontWeight: '500'
    lineHeight: 18px
  metadata:
    fontFamily: Inter
    fontSize: 12px
    fontWeight: '400'
    lineHeight: 16px
  page-title-mobile:
    fontFamily: Inter
    fontSize: 18px
    fontWeight: '600'
    lineHeight: 24px
rounded:
  sm: 0.125rem
  DEFAULT: 0.25rem
  md: 0.375rem
  lg: 0.5rem
  xl: 0.75rem
  full: 9999px
spacing:
  header-height: 56px
  sidebar-width: 240px
  container-padding: 24px
  gap-xs: 4px
  gap-sm: 8px
  gap-md: 16px
  gap-lg: 24px
  base-unit: 4px
---

## Brand & Style
The design system is engineered for occupational health administration, prioritizing clarity, efficiency, and medical authority. The target audience includes clinicians, HR administrators, and health safety officers who require a high-density, low-friction interface to manage complex employee health data.

The style is **Corporate / Modern** with a focus on data density and systematic organization. It utilizes a structured layout to maintain a sense of calm and control despite the high volume of information. The aesthetic is clean and functional, employing a "neutral-plus" approach where color is reserved strictly for semantic meaning (status, urgency) and primary actions.

## Colors
The palette is anchored by a medical "Teal" primary color that communicates technology and care. A comprehensive neutral scale is used to define the structural hierarchy, with `Neutral 100` serving as the canvas background to reduce eye strain during long-form data entry. 

Semantic colors (Success, Warning, Error) are paired with ultra-light backgrounds to create high-visibility status indicators that don't overwhelm the visual field. This design system relies on consistent color-coding for medical compliance and urgency levels.

## Typography
The typography system uses **Inter** exclusively to ensure maximum legibility across data-dense tables and complex forms. By utilizing a slightly smaller base size of 13px, the design system allows for more information to be visible on-screen without sacrificing clarity. 

Section headings and Page titles use semi-bold weights (`600`) to provide strong anchors for the user's eye. Metadata and small labels are reserved for secondary information like timestamps or helper text.

## Layout & Spacing
The design system employs a **Fixed Grid** philosophy for its primary navigation and a fluid content area for data visualization. 

- **Top Header:** 56px height, fixed to top, `White` background with a `Neutral 200` bottom border.
- **Left Sidebar:** 240px width, fixed to left, `Neutral 900` or high-contrast dark background to distinguish navigation from content.
- **Content Area:** Sets on a `Neutral 100` background. Standard padding is 24px across all viewport sizes, though it scales down to 16px on mobile.
- **Grid:** Use a 12-column grid within the content area. KPI cards typically span 3 columns (4 per row) on desktop and 12 columns on mobile.

## Elevation & Depth
Depth is conveyed through **Tonal Layers** and subtle outlines rather than heavy shadows, maintaining a clean, professional aesthetic.

- **Level 0 (Surface):** `Neutral 100` — The background for the entire application.
- **Level 1 (Cards/Tables):** `White` — Used for the main content containers. These should feature a 1px solid border in `Neutral 200` to define boundaries.
- **Level 2 (Dropdowns/Modals):** `White` with a soft ambient shadow (0px 4px 12px rgba(0,0,0,0.05)) and a `Neutral 200` border.

Interactive elements use a focus ring of 2px `Primary Light` to indicate keyboard navigation and active state.

## Shapes
This design system uses a **Soft** shape language (`0.25rem` / 4px) to balance the clinical precision of the interface with a modern, approachable feel. 

- **Standard Elements:** Inputs, buttons, and cards use the base 4px radius.
- **Pill Elements:** Status chips and tags use a fully rounded (pill) shape to distinguish them from interactive buttons.

## Components

### Buttons & Inputs
- **Primary Button:** `Primary Teal` background, `White` text, 4px border-radius.
- **Form Fields:** `White` background, `Neutral 200` border. On focus, use a `Primary Teal` border and a `Primary Light` outer glow.

### KPI Stat Cards
- White background with a 1px `Neutral 200` border.
- Include a 4px left-accent border using the `Primary Teal` or relevant client-specific color to visually categorize metrics.

### Data Tables
- Header row: `Neutral 50` background, `Neutral 600` uppercase text (12px metadata style).
- Row hover state: `Neutral 50` background.
- Status Chips: Pill-shaped with semantic background colors (e.g., `Success Light`) and corresponding dark text (`Success Base/Dark`).

### Charts
- **Donut/Bar/Line:** Use a sequence of `Primary Base`, `Success Base`, `Warning Base`, and `Primary Dark` to ensure consistent data visualization. Chart grid lines should use `Neutral 200`.

### Navigation
- **Sidebar Links:** High-contrast `White` text on `Neutral 900`. Active state should include a `Primary Teal` vertical indicator on the left edge.