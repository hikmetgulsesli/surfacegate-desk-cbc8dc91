---
name: Precision Logic System
colors:
  surface: '#f8f9ff'
  surface-dim: '#cbdbf5'
  surface-bright: '#f8f9ff'
  surface-container-lowest: '#ffffff'
  surface-container-low: '#eff4ff'
  surface-container: '#e5eeff'
  surface-container-high: '#dce9ff'
  surface-container-highest: '#d3e4fe'
  on-surface: '#0b1c30'
  on-surface-variant: '#45464d'
  inverse-surface: '#213145'
  inverse-on-surface: '#eaf1ff'
  outline: '#76777d'
  outline-variant: '#c6c6cd'
  surface-tint: '#565e74'
  primary: '#000000'
  on-primary: '#ffffff'
  primary-container: '#131b2e'
  on-primary-container: '#7c839b'
  inverse-primary: '#bec6e0'
  secondary: '#4648d4'
  on-secondary: '#ffffff'
  secondary-container: '#6063ee'
  on-secondary-container: '#fffbff'
  tertiary: '#000000'
  on-tertiary: '#ffffff'
  tertiary-container: '#271901'
  on-tertiary-container: '#98805d'
  error: '#ba1a1a'
  on-error: '#ffffff'
  error-container: '#ffdad6'
  on-error-container: '#93000a'
  primary-fixed: '#dae2fd'
  primary-fixed-dim: '#bec6e0'
  on-primary-fixed: '#131b2e'
  on-primary-fixed-variant: '#3f465c'
  secondary-fixed: '#e1e0ff'
  secondary-fixed-dim: '#c0c1ff'
  on-secondary-fixed: '#07006c'
  on-secondary-fixed-variant: '#2f2ebe'
  tertiary-fixed: '#fcdeb5'
  tertiary-fixed-dim: '#dec29a'
  on-tertiary-fixed: '#271901'
  on-tertiary-fixed-variant: '#574425'
  background: '#f8f9ff'
  on-background: '#0b1c30'
  surface-variant: '#d3e4fe'
typography:
  headline-lg:
    fontFamily: Inter
    fontSize: 24px
    fontWeight: '700'
    lineHeight: 32px
    letterSpacing: -0.02em
  headline-md:
    fontFamily: Inter
    fontSize: 20px
    fontWeight: '600'
    lineHeight: 28px
    letterSpacing: -0.01em
  headline-sm:
    fontFamily: Inter
    fontSize: 16px
    fontWeight: '600'
    lineHeight: 24px
  body-lg:
    fontFamily: Inter
    fontSize: 16px
    fontWeight: '400'
    lineHeight: 24px
  body-md:
    fontFamily: Inter
    fontSize: 14px
    fontWeight: '400'
    lineHeight: 20px
  body-sm:
    fontFamily: Inter
    fontSize: 13px
    fontWeight: '400'
    lineHeight: 18px
  label-md:
    fontFamily: Inter
    fontSize: 12px
    fontWeight: '600'
    lineHeight: 16px
    letterSpacing: 0.05em
  label-sm:
    fontFamily: Inter
    fontSize: 11px
    fontWeight: '500'
    lineHeight: 14px
  headline-lg-mobile:
    fontFamily: Inter
    fontSize: 20px
    fontWeight: '700'
    lineHeight: 28px
rounded:
  sm: 0.125rem
  DEFAULT: 0.25rem
  md: 0.375rem
  lg: 0.5rem
  xl: 0.75rem
  full: 9999px
spacing:
  base: 4px
  xs: 4px
  sm: 8px
  md: 16px
  lg: 24px
  xl: 32px
  gutter: 12px
  margin-mobile: 16px
  margin-desktop: 24px
---

## Brand & Style

The design system is engineered for high-performance service environments where information density is a requirement, not a choice. The brand personality is **authoritative, composed, and hyper-efficient**. It prioritizes a "Dense but Calm" aesthetic, ensuring that even when the screen is flooded with data, the user feels in control through logical grouping and rhythmic spacing.

The style is **Corporate / Modern** with a lean toward **Functional Minimalism**. It avoids decorative flourishes in favor of structural integrity. Every pixel must serve a purpose, reducing cognitive load for service desk agents who spend hours within the interface. The emotional response should be one of "reliable precision."

## Colors

The palette is anchored by **Deep Navy** (#0F172A) for structural navigation and primary text, providing a stable foundation. **Indigo** (#6366F1) serves as the primary action color, offering high visibility against the neutral background.

Functional status colors are critical for this design system:
- **Emerald (#059669):** Used for 'Resolved' or 'Success' states to signal completion without being jarring.
- **Amber (#D97706):** Used for 'Pending' or 'Warning' states to draw attention to items requiring follow-up.
- **Indigo (#4F46E5):** Reserved for 'Open' or 'Active' states, distinguishing them from standard UI actions.

The background uses a subtle **Slate Gray** wash (#F8FAFC) to separate the canvas from the crisp white (#FFFFFF) card containers, creating a clear visual hierarchy of information layers.

## Typography

This design system utilizes **Inter** for all roles to leverage its exceptional legibility at small sizes. The typographic scale is tightly controlled to facilitate high density.

- **Data Views:** Use `body-sm` for table rows and list items to maximize content visibility.
- **Metadata:** `label-md` is used for category headers and table columns, using a slight uppercase treatment to differentiate from dynamic content.
- **Readability:** Despite the density, line heights are never less than 1.4x the font size to ensure the eye can easily track across long rows of data.

## Layout & Spacing

The layout is built on a **strict 4px grid base**. This allows for the "Dense but Calm" requirement, providing enough granularity to tuck elements together while maintaining mathematical alignment.

**Grid System:**
- **Desktop:** A 12-column fluid grid with 12px gutters. The narrower gutters facilitate higher information density across the horizontal plane.
- **Tablet:** A 6-column grid with 16px margins.
- **Mobile:** A 2-column grid or single-stack layout with 16px margins.

Layout logic relies heavily on **Auto-layout containers** with consistent `8px` or `12px` internal padding for cards. Sections of the interface should be separated by clear 1px borders to define boundaries without wasting the space that large margins would require.

## Elevation & Depth

To maintain a clean, professional aesthetic, this design system uses **Low-contrast outlines** as the primary method of separation, supplemented by **Ambient shadows** only for interactive or floating elements.

- **Flat Layer:** The main canvas background.
- **Level 1 (Cards/Sidebar):** 1px border (#E2E8F0) with no shadow. This keeps the density high and the look "flat" and organized.
- **Level 2 (Dropdowns/Modals/Hover States):** A soft, diffused shadow (0px 4px 6px -1px rgba(0, 0, 0, 0.1)) to indicate the element is temporarily layered above the grid.
- **Interactive Focus:** Elements should never use "glow" effects. Instead, use a 2px solid border in the primary Indigo color for accessibility-compliant focus states.

## Shapes

The shape language is **Soft (Level 1)**. 

- **Components:** Buttons, input fields, and chips use a `4px` (0.25rem) radius. This provides a modern touch without the "child-like" feel of large rounded corners.
- **Containers:** Large cards and modals use `8px` (0.5rem) to slightly soften the structural edges of the dashboard.
- **Status Indicators:** Use the same `4px` radius or, in the case of status dots, a full circle.

The consistent use of small radii maintains the professional, architectural feel required for a service desk interface.

## Components

### Buttons
Primary buttons use a solid Deep Navy or Indigo fill with white text. Secondary buttons use a 1px border with neutral text. All buttons have a fixed height of `32px` for standard density and `40px` for primary page actions.

### Status Chips
Status chips are critical. They use a light background tint of the status color with high-contrast text (e.g., Emerald text on a 10% opacity Emerald background). This ensures they are readable but do not visually overwhelm the actual data.

### Input Fields
Inputs use a `1px` border (#CBD5E1). In a dense environment, labels should be placed above the field in `label-sm` style to save horizontal space. Height is standardized at `32px` to match buttons.

### Data Tables
The heart of the system. Rows are `40px` tall. Zebra striping is avoided in favor of subtle `1px` bottom borders (#F1F5F9). The "Hover" state on a row should apply a very light gray (#F8FAFC) background and a visible "Quick Action" menu at the end of the row.

### Cards
Cards are the primary container for grouping related ticket information. They feature a `1px` border, no shadow (unless hovered), and a `12px` internal padding to balance density with legibility.