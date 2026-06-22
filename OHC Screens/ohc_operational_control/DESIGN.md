---
name: OHC Operational Control
colors:
  surface: '#f6faff'
  surface-dim: '#d6dae0'
  surface-bright: '#f6faff'
  surface-container-lowest: '#ffffff'
  surface-container-low: '#f0f4fa'
  surface-container: '#eaeef4'
  surface-container-high: '#e4e8ee'
  surface-container-highest: '#dee3e9'
  on-surface: '#171c20'
  on-surface-variant: '#3e4850'
  inverse-surface: '#2c3135'
  inverse-on-surface: '#edf1f7'
  outline: '#6e7881'
  outline-variant: '#bec8d2'
  surface-tint: '#006591'
  primary: '#006591'
  on-primary: '#ffffff'
  primary-container: '#0ea5e9'
  on-primary-container: '#003751'
  inverse-primary: '#89ceff'
  secondary: '#3c627d'
  on-secondary: '#ffffff'
  secondary-container: '#b8dffe'
  on-secondary-container: '#3d637d'
  tertiary: '#8a5100'
  on-tertiary: '#ffffff'
  tertiary-container: '#de8712'
  on-tertiary-container: '#4d2b00'
  error: '#ba1a1a'
  on-error: '#ffffff'
  error-container: '#ffdad6'
  on-error-container: '#93000a'
  primary-fixed: '#c9e6ff'
  primary-fixed-dim: '#89ceff'
  on-primary-fixed: '#001e2f'
  on-primary-fixed-variant: '#004c6e'
  secondary-fixed: '#c9e6ff'
  secondary-fixed-dim: '#a5cbe9'
  on-secondary-fixed: '#001e2f'
  on-secondary-fixed-variant: '#234b64'
  tertiary-fixed: '#ffdcbd'
  tertiary-fixed-dim: '#ffb86e'
  on-tertiary-fixed: '#2c1600'
  on-tertiary-fixed-variant: '#693c00'
  background: '#f6faff'
  on-background: '#171c20'
  surface-variant: '#dee3e9'
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
  label-sm:
    fontFamily: Inter
    fontSize: 11px
    fontWeight: '600'
    lineHeight: 16px
rounded:
  sm: 0.25rem
  DEFAULT: 0.5rem
  md: 0.75rem
  lg: 1rem
  xl: 1.5rem
  full: 9999px
spacing:
  base: 4px
  header_height: 56px
  sidebar_width: 240px
  content_padding: 24px
  container_gap: 16px
  stack_gap: 8px
---

## Brand & Style
The design system is engineered for high-density enterprise environments where precision, compliance, and clarity are paramount. The brand personality is authoritative, reliable, and systematic. 

The visual style follows a **Corporate / Modern** aesthetic, prioritizing functional efficiency over decorative flair. It utilizes a structured grid, clear information hierarchy, and a restrained color palette to reduce cognitive load for users managing complex data sets. Every element is intentional, creating a "locked" and disciplined interface that evokes a sense of institutional stability and professional rigor.

## Colors
The palette is dominated by the Primary Teal for action states and the Primary Dark for structural branding. A comprehensive neutral scale from 50 to 900 provides the necessary nuance for borders, backgrounds, and varying levels of text emphasis. 

Semantic colors are strictly reserved for status indicators:
- **Active/Compliant**: Emerald-based tints.
- **Pending**: Amber-based tints.
- **Inactive/Overdue**: Rose-based tints.
The default background for the application canvas is Neutral 50, providing a subtle contrast against white container elements.

## Typography
This design system utilizes **Inter** exclusively to ensure maximum legibility across all display types. The type scale is intentionally compact to facilitate high-density data presentation. 

- **Page Titles**: Used for primary module headers.
- **Section Headings**: Used for card titles and subsection labeling.
- **Body**: The standard for all data entries and descriptive text.
- **Label SM**: Used for table headers and small metadata, often paired with a subtle letter-spacing increase for readability.

## Layout & Spacing
The layout relies on a **Fixed Sidebar and Header** model to provide a persistent navigation anchor. 

- **Header**: Fixed at 56px height, containing global search and user profile.
- **Sidebar**: Fixed at 240px width, containing the primary navigation tree.
- **Content Area**: A fluid region with a mandatory 24px internal padding on all sides.

All internal component spacing follows a 4px base unit. Elements within cards or forms should typically utilize 8px (2 units) or 16px (4 units) increments to maintain a rigorous vertical and horizontal rhythm.

## Elevation & Depth
This design system minimizes the use of shadows to maintain a clean, "flat" enterprise appearance. Depth is primarily communicated through **Low-contrast outlines** and **Tonal layers**.

- **Level 0**: Background canvas (Neutral 50).
- **Level 1**: Cards and primary containers (White with Neutral 200 border).
- **Level 2**: Modals and popovers (White with a soft 4px blur shadow, 10% opacity black).

Borders are the primary method of separation. Use 1px solid #E5E7EB (Neutral 200) for all standard container boundaries.

## Shapes
A consistent **8px (0.5rem)** radius is applied to all primary UI elements, including cards, input fields, and buttons. This creates a "Rounded" aesthetic that feels modern but remains professional. Small elements like checkboxes or status tags may use a reduced 4px radius where appropriate, while status chips may optionally use a full pill-shape for distinct visual categorization.

## Components
### Buttons
- **Height**: Fixed at 36px.
- **Primary**: Solid #0EA5E9 background, white text.
- **Secondary**: Outlined with Neutral 200, Primary Dark text.
- **Padding**: 16px horizontal.

### Cards
- **Surface**: White background.
- **Border**: 1px solid #E5E7EB.
- **Corner Radius**: 8px.
- **Padding**: 20px or 24px depending on content density.

### Tables
- **Rows**: Alternating (striped). Even rows White (#FFFFFF), odd rows Neutral 50 (#FAFAFA).
- **Cells**: 12px vertical and horizontal padding.
- **Header**: Neutral 50 background, 1px bottom border, bold Label-SM typography.

### Status Chips
- **Geometry**: 24px height, 12px horizontal padding.
- **Colors**: Use the semantic background/text pairs defined in the color section. Text is centered and semibold.

### Inputs
- **Height**: 36px.
- **Border**: 1px solid #D1D5DB.
- **Focus State**: 1px solid #0EA5E9 with a subtle 2px outer glow.