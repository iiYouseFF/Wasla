# Design System & Style Guide
## B2B FMCG Marketplace App

**Version:** 1.0
**Status:** Draft — token values are placeholders for a designer/brand team to finalize

---

## 1. Purpose

Defines the shared visual language and component rules used across the Retailer app, Supplier/Wholesaler dashboard, Sales Agent app, and Admin panel, so all four surfaces feel like one product and frontend teams can build consistent, reusable components.

---

## 2. Brand Foundations

### 2.1 Color Palette (placeholder tokens — finalize with brand designer)

| Token | Usage | Placeholder Value |
|---|---|---|
| `color-primary` | Primary actions, links, active states | `#1B5E20` (deep green — evokes trade/growth; replace with brand color) |
| `color-primary-dark` | Pressed/hover state of primary | `#144517` |
| `color-secondary` | Secondary accents, highlights | `#F5A623` |
| `color-success` | Confirmed/delivered/positive states | `#2E7D32` |
| `color-warning` | Pending/low-stock/attention states | `#F9A825` |
| `color-danger` | Errors, cancellations, overdue BNPL | `#C62828` |
| `color-neutral-900` | Primary text | `#1A1A1A` |
| `color-neutral-600` | Secondary text | `#5F5F5F` |
| `color-neutral-300` | Borders, dividers | `#D9D9D9` |
| `color-neutral-100` | Backgrounds, cards | `#F7F7F7` |
| `color-surface` | App background | `#FFFFFF` |

**Status badge color mapping** (used across order/account states):
- `pending` → neutral/warning
- `confirmed` / `active` → primary
- `out_for_delivery` → secondary
- `delivered` / `completed` / `approved` → success
- `cancelled` / `rejected` / `overdue` → danger

### 2.2 Typography

| Token | Usage | Placeholder |
|---|---|---|
| `font-family` | All text | System font stack (San Francisco/Roboto) or a brand-licensed font — confirm Arabic-compatible font pairing |
| `font-heading-xl` | Screen titles | 24px / Bold |
| `font-heading-md` | Section headers | 18px / Semibold |
| `font-body` | Body text | 14px / Regular |
| `font-caption` | Helper text, timestamps | 12px / Regular |
| `font-button` | Button labels | 14px / Semibold |

**RTL requirement:** Typography and layout must mirror correctly for Arabic. Test all components in both LTR and RTL from day one — this is not a late-stage add-on given the target market.

### 2.3 Spacing Scale

| Token | Value |
|---|---|
| `space-xs` | 4px |
| `space-sm` | 8px |
| `space-md` | 16px |
| `space-lg` | 24px |
| `space-xl` | 32px |

### 2.4 Corner Radius & Elevation

| Token | Value |
|---|---|
| `radius-sm` | 4px (inputs, badges) |
| `radius-md` | 8px (cards, buttons) |
| `radius-lg` | 16px (modals, bottom sheets) |
| `elevation-card` | subtle shadow, `0 1px 3px rgba(0,0,0,0.08)` |
| `elevation-modal` | `0 4px 16px rgba(0,0,0,0.16)` |

---

## 3. Core Components

### 3.1 Button
- Variants: `primary`, `secondary`, `outline`, `text`, `destructive`
- States: default, hover (web), pressed, disabled, loading (spinner inline)
- Sizes: `sm`, `md` (default), `lg` (used for sticky CTA buttons like "Place Order")

### 3.2 Input Field
- Variants: text, phone (with country code prefix), OTP (auto-advancing 6-box input), number/quantity stepper, dropdown/select
- States: default, focused, error (red border + helper text below), disabled
- Always paired with a label above and optional helper/error text below

### 3.3 Card
- Product card (image, name, price, seller, CTA)
- Order card (order #, status badge, item count, total, date)
- Used consistently across list views in all four apps

### 3.4 Status Badge
- Pill-shaped, color per status mapping (§2.1)
- Always paired with readable text label, never color-only (accessibility)

### 3.5 Navigation
- Mobile: bottom tab bar (5 items max — Home, Categories, Cart, Orders, Profile for Retailer app)
- Web: left sidebar nav (collapsible on smaller viewports)

### 3.6 Modal / Bottom Sheet
- Mobile: bottom sheet for quick actions (filters, quantity edit)
- Web: centered modal for forms (add product, approve account)

### 3.7 Empty States
- Illustration + short message + primary CTA (e.g., "Your cart is empty" → "Browse Products")

### 3.8 Toast / Snackbar
- Auto-dismiss after 3-4s, used for non-blocking confirmations ("Item added to cart")

### 3.9 Loading States
- Skeleton screens for content-heavy views (product lists, order history)
- Spinner for button-level async actions

---

## 4. Iconography

- Use a single consistent icon set across all apps (recommend an open-source set like Feather/Lucide or a custom icon font matching brand)
- Icons must have accessible labels (not icon-only buttons without a text alternative or tooltip)

---

## 5. Accessibility Baseline

- Minimum tap target size: 44x44px (mobile)
- Color contrast: minimum WCAG AA (4.5:1 for body text)
- All interactive elements reachable via screen reader with meaningful labels
- Never rely on color alone to convey status (pair with text/icon)

---

## 6. Localization & Content Rules

- All UI strings externalized to translation files from day one (no hardcoded text in components)
- Arabic and English supported at launch; layout direction switches automatically with locale
- Currency and date formatting locale-aware, not hardcoded
- Error messages should be actionable and specific (e.g., "Minimum order is 5 units" not "Invalid quantity")

---

## 7. Next Steps

- Finalize actual brand colors/typography with a designer or brand agency (values above are structural placeholders)
- Build out a Figma component library matching these tokens before high-fidelity screen design begins
- Once Figma library exists, generate a shared code-level design token file (e.g., a `tokens.json` or Tailwind config) consumed by both React Native and React web apps for consistency
