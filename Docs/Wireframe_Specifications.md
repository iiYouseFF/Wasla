# Wireframe Specifications
## B2B FMCG Marketplace App — MVP Screens

**Version:** 1.0
**Status:** Draft
**Purpose:** Layout-level specs for each MVP screen, to hand to a designer for high-fidelity mockups (Figma) or to brief a frontend dev directly. These are structural/content specs, not visual designs.

---

## 1. Retailer App — Home Screen

**Layout (top to bottom):**
1. Header: business name/greeting, notification bell icon, cart icon (with item count badge)
2. Search bar (tappable, opens Search screen)
3. Category quick-tiles (horizontal scroll, icon + label, 6-8 top categories)
4. Promotions carousel (auto-rotating banners, tappable → Promotion Detail)
5. "Recently Ordered" horizontal product carousel
6. "Recommended for You" horizontal product carousel
7. Bottom tab bar: Home | Categories | Cart | Orders | Profile

**States:** loading skeleton, empty state (no recent orders → hide that section), offline banner if no connection

---

## 2. Retailer App — Product Detail Screen

**Layout:**
1. Image carousel (product photos)
2. Product name, brand, unit of measure
3. Price + "Compare sellers" link (expands to Seller Comparison view)
4. Seller name, rating, stock status ("In stock" / "Only 12 left" / "Out of stock")
5. Minimum order quantity notice (if applicable)
6. Quantity stepper + "Add to Cart" button (sticky at bottom)
7. Product description/specs (expandable section)
8. Reviews summary (average rating + count, tappable to full reviews list)

---

## 3. Retailer App — Cart Screen

**Layout:**
1. Header: "Cart" title, item count
2. List grouped by seller (collapsible sections):
   - Seller name header + MOQ status badge (met/not met)
   - Line items: product image thumbnail, name, unit price, quantity stepper, remove icon
   - Seller subtotal
3. Grand total (sticky footer)
4. "Proceed to Checkout" button (disabled/greyed if any seller group hasn't met MOQ, with inline warning text)

**Empty state:** illustration + "Your cart is empty" + "Browse products" CTA

---

## 4. Retailer App — Checkout Screen

**Layout:**
1. Delivery address section (selected address card + "Change" link)
2. Payment method selector (radio list: Cash on Delivery, Card, Wallet, BNPL — BNPL shows available credit inline, disabled if insufficient)
3. Order summary (collapsed by default, expandable — items grouped by seller)
4. Total breakdown: subtotal, delivery fee (if any), total
5. "Place Order" button (sticky footer)

---

## 5. Retailer App — Order Tracking Screen

**Layout:**
1. Header: Order # and status label
2. Horizontal status stepper: Placed → Confirmed → Packed → Out for Delivery → Delivered
3. Map view (if out for delivery) showing driver location + ETA
4. Sub-order breakdown cards (one per seller, each with its own mini status if order spans multiple sellers)
5. "Contact Support" button
6. "Reorder" button (visible once delivered)

---

## 6. Supplier Dashboard — Order Management Screen (Web)

**Layout (desktop, 2-column):**
- Left sidebar: nav (Dashboard, Catalog, Orders, Promotions, Analytics, Settings)
- Main content:
  1. Filter bar: status tabs (Pending, Confirmed, Packed, Out for Delivery, Completed, Cancelled), date range picker, search by order ID
  2. Orders table: Order ID, Retailer name, Items count, Total, Status badge, Date, Action button
  3. Row click → Order Detail drawer/modal (item list, retailer info, delivery address, status update dropdown)
  4. Pagination footer

---

## 7. Supplier Dashboard — Catalog Management Screen (Web)

**Layout:**
1. "Add Product" button (top right)
2. Product table: image thumbnail, name, category, price, stock, MOQ, status (active/inactive toggle), edit icon
3. Bulk actions: bulk price update, CSV import/export buttons
4. Add/Edit Product modal: name, category dropdown, brand, unit of measure, image upload, price, MOQ, stock quantity, active toggle

---

## 8. Admin Panel — Account Approval Queue (Web)

**Layout:**
1. Tabs: Pending, Approved, Rejected
2. Table: Business name, Role (Supplier/Wholesaler), Submitted date, KYC docs (view link), Approve/Reject buttons
3. Row click → detail panel with full KYC document viewer + approval notes field

---

## 9. Common Components Needed (Design System Inputs)

- Button (primary, secondary, disabled states)
- Input field (text, phone, OTP-specific with auto-advance)
- Status badge (color-coded per order/account status)
- Card (product card, order card)
- Bottom sheet / modal
- Toast/snackbar for confirmations
- Empty state illustration + text pattern
- Loading skeleton pattern
- Stepper/quantity selector
- Tab bar (mobile) / sidebar nav (web)

---

## 10. Next Step

These specs are ready to hand to a UI/UX designer to produce high-fidelity mockups (Figma recommended) using the design system tokens once defined (colors, typography, spacing — see Design System doc, still to be created). Recommend designing mobile screens mobile-first, then adapting shared components to web dashboards.
