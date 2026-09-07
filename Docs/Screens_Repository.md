# Screens Repository
## B2B FMCG Marketplace App

**Version:** 1.0
**Status:** Draft

This document catalogs every screen needed across the platform's four client apps, organized by flow. Use this as the backlog for wireframing/UI design and frontend ticket breakdown.

---

## 1. Retailer Mobile App

### 1.1 Onboarding & Auth
- Splash screen
- Language selection (Arabic/English toggle)
- Phone number entry
- OTP verification
- Business profile setup (business name, type, address, location pin)
- Welcome/tutorial carousel (optional, first-time only)

### 1.2 Home & Discovery
- Home screen (categories, banners/promotions, recently ordered, recommended products)
- Category listing screen
- Subcategory/brand filter screen
- Search screen (with autocomplete + recent searches)
- Search results / filtered listing screen
- Product detail screen (price, unit, seller info, stock status, reviews)
- Seller comparison view (same product, multiple sellers/prices)

### 1.3 Cart & Checkout
- Cart screen (grouped by seller/supplier)
- Edit quantity / remove item (inline in cart)
- Checkout screen (delivery address confirmation, payment method selection)
- BNPL eligibility/limit display (if applicable at checkout)
- Order summary/review screen
- Order confirmation screen

### 1.4 Orders
- Order history list (filter by status: active, completed, cancelled)
- Order detail screen (items, sub-order breakdown per seller, status timeline)
- Order tracking screen (live delivery status/map)
- Reorder confirmation screen
- Cancel/return order flow

### 1.5 Payments & BNPL
- Payment methods management screen
- Add card / wallet screen
- BNPL account overview (credit limit, available credit, repayment schedule)
- BNPL repayment screen
- Transaction/payment history

### 1.6 Reviews & Support
- Rate & review screen (post-delivery)
- Help/support center
- Contact support / chat screen
- Dispute/report an issue screen

### 1.7 Profile & Settings
- Profile screen (business info edit)
- Notification preferences
- Language settings
- Saved addresses
- Logout/account management

### 1.8 Notifications
- Notification inbox/list screen
- Promotion detail screen (from notification deep-link)

---

## 2. Supplier / Wholesaler Web Dashboard

### 2.1 Onboarding & Auth
- Login screen
- Registration/signup screen
- KYC document upload screen
- Account pending approval screen
- Business profile setup

### 2.2 Catalog Management
- Product list screen
- Add/edit product screen (name, category, images, unit of measure)
- Listing management screen (price, stock, MOQ per product)
- Bulk upload screen (CSV import for catalog)
- Category browser (assign products to categories)

### 2.3 Inventory
- Inventory overview screen (stock levels across warehouses)
- Stock adjustment screen (stock-in/stock-out entry)
- Low-stock alerts screen
- Warehouse/location management screen

### 2.4 Orders
- Incoming orders list (filter by status)
- Order detail screen (items, retailer info, delivery info)
- Order status update screen (confirm, pack, dispatch)
- Order history/archive screen

### 2.5 Promotions
- Promotions list screen
- Create/edit promotion screen (discount type, target listings, date range)
- Promotion performance screen

### 2.6 Analytics & Reporting
- Sales dashboard (GMV, top products, trend charts)
- Demand heatmap by region
- Payout history screen
- Downloadable reports screen

### 2.7 Settings
- Account/profile settings
- Payout account details
- Team/user management (multiple staff logins per supplier account)
- Notification preferences

---

## 3. Sales Agent Mobile App

### 3.1 Auth & Home
- Login screen
- Home/dashboard (assigned retailers, today's visit schedule)

### 3.2 Retailer Onboarding
- New retailer registration screen (agent-assisted)
- Business info capture screen
- Location pin/verification screen
- Onboarding confirmation screen

### 3.3 Retailer Management
- My retailers list screen
- Retailer detail screen (order history, credit status)
- Visit check-in screen (geolocation-based)
- Visit notes/log screen

### 3.4 Assisted Ordering
- Place order on behalf of retailer screen
- Product browsing (shared component with retailer app)
- Order confirmation (agent-assisted)

### 3.5 Performance
- Agent performance dashboard (retailers onboarded, orders driven, targets)

---

## 4. Admin Web Panel

### 4.1 Auth
- Admin login screen (with 2FA)

### 4.2 Dashboard
- Platform overview dashboard (GMV, active users, order volume, key KPIs)

### 4.3 User Management
- Retailer accounts list/search
- Supplier/wholesaler accounts list/search
- Account approval queue (KYC review)
- Account detail/edit screen
- Suspend/ban account screen

### 4.4 Catalog Moderation
- Product listing moderation queue
- Category management screen
- Flagged/reported listings screen

### 4.5 Order Operations
- All orders list (searchable/filterable)
- Order detail/intervention screen
- Dispute management screen
- Refund processing screen

### 4.6 Financial Operations
- Payout management screen (batch approve/process)
- BNPL account overview & risk dashboard
- Transaction reconciliation screen

### 4.7 Marketing & Promotions
- Platform-wide promotion management
- Banner/content management screen

### 4.8 Reports & Analytics
- Custom report builder
- Export data screen

### 4.9 System Settings
- Role & permission management
- City/region configuration
- Notification template management

---

## 5. Shared/Cross-Cutting Screens

- Error states (no connection, server error, empty states)
- Loading/skeleton screens
- Generic confirmation/success modal
- Terms of Service / Privacy Policy screen
- App update prompt screen

---

## 6. Suggested Prioritization for MVP

**Must-have for MVP (Phase 1):**
- Retailer: onboarding, home/browse, cart/checkout (COD only), order tracking, order history
- Supplier: login, catalog management, order management
- Admin: user approval, order operations, basic dashboard

**Defer to Phase 2/3:**
- BNPL screens (retailer + admin risk dashboard)
- Promotions (retailer-facing + supplier creation)
- Sales agent app (if launch relies on manual/other onboarding initially)
- Advanced analytics/reporting screens
