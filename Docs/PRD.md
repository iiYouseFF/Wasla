# Product Requirements Document (PRD)
## B2B FMCG Marketplace App ("the Platform")

**Version:** 1.0
**Status:** Draft
**Inspired by:** Cartona (Egypt B2B marketplace model)

---

## 1. Overview

### 1.1 Problem Statement
Small retailers (corner shops, mini-markets, cafes, restaurants) currently procure FMCG (Fast-Moving Consumer Goods) inventory through fragmented, inefficient channels: informal wholesalers, manual phone/in-person orders, cash-only payments, and unreliable delivery. This costs retailers time and money and limits their growth. Meanwhile, suppliers and wholesalers lack visibility into real-time demand and efficient distribution channels to reach the long tail of small retailers.

### 1.2 Vision
Build a B2B e-commerce marketplace that digitizes the traditional trade supply chain — connecting retailers directly to FMCG suppliers and wholesalers through a mobile-first ordering platform, with embedded financing and real-time inventory/order management.

### 1.3 Goals
- Give retailers a 24/7 self-service channel to browse, compare, and order FMCG products
- Give suppliers/wholesalers a digital demand-aggregation and fulfillment channel
- Reduce order-to-delivery friction and provide real-time order tracking
- Enable embedded finance (Buy Now, Pay Later) to increase retailer purchasing power
- Provide data-driven insights to suppliers for inventory and marketing decisions

### 1.4 Non-Goals (v1)
- Consumer-facing (B2C) storefront
- Cross-border/international shipping
- Full-scale in-house logistics fleet (v1 assumes asset-light model — 3rd-party or partner delivery)
- Complex financial lending underwriting engine (BNPL v1 can integrate a 3rd-party fintech provider rather than build in-house)

---

## 2. User Personas & Roles

| Role | Description | Primary Platform |
|---|---|---|
| **Retailer** | Small shop/cafe/restaurant owner or staff who buys FMCG products | Mobile app (iOS/Android) |
| **Supplier (FMCG Brand)** | Manufacturer or brand wanting demand aggregation & sales data | Web dashboard |
| **Wholesaler** | Distributor holding inventory who fulfills orders | Web dashboard + mobile (optional) |
| **Sales Agent / Field Rep** | Internal team member onboarding retailers, supporting orders | Mobile app (lightweight CRM) |
| **Platform Admin / Ops** | Internal staff managing catalog, disputes, payouts, logistics | Web admin panel |

---

## 3. User Stories & Requirements

### 3.1 Retailer App

| ID | User Story | Priority |
|---|---|---|
| R1 | As a retailer, I can register/login using phone number + OTP | Must |
| R2 | As a retailer, I can browse products by category, brand, or search | Must |
| R3 | As a retailer, I can view product price, unit size, stock availability | Must |
| R4 | As a retailer, I can compare prices for the same product across suppliers/wholesalers | Should |
| R5 | As a retailer, I can add products to cart and place an order | Must |
| R6 | As a retailer, I can view my order history and reorder past orders | Must |
| R7 | As a retailer, I can track order status (placed, confirmed, out for delivery, delivered) | Must |
| R8 | As a retailer, I can pay via cash on delivery, card, wallet, or BNPL | Must |
| R9 | As a retailer, I can view my BNPL credit limit and repayment schedule | Should |
| R10 | As a retailer, I can rate/review a supplier or delivery experience | Could |
| R11 | As a retailer, I receive push notifications for order updates and promotions | Must |
| R12 | As a retailer, I can view ongoing promotions/discounts | Should |

### 3.2 Supplier / Wholesaler Dashboard

| ID | User Story | Priority |
|---|---|---|
| S1 | As a supplier, I can create and manage my product catalog (SKUs, pricing, images) | Must |
| S2 | As a supplier, I can view and manage incoming orders | Must |
| S3 | As a supplier, I can update inventory levels in real time | Must |
| S4 | As a supplier, I can view sales analytics (top products, demand by region) | Must |
| S5 | As a supplier, I can create targeted promotions/discounts | Should |
| S6 | As a wholesaler, I can use a lightweight ERP view for stock-in/stock-out tracking | Should |
| S7 | As a supplier, I can view payout/settlement history | Must |

### 3.3 Sales Agent App

| ID | User Story | Priority |
|---|---|---|
| A1 | As a sales agent, I can onboard new retailers in the field | Must |
| A2 | As a sales agent, I can place orders on behalf of a retailer | Should |
| A3 | As a sales agent, I can view my assigned retailer list and visit schedule | Should |

### 3.4 Admin Panel

| ID | User Story | Priority |
|---|---|---|
| AD1 | As an admin, I can approve/reject new supplier/wholesaler accounts | Must |
| AD2 | As an admin, I can moderate product listings | Must |
| AD3 | As an admin, I can manage order disputes and refunds | Must |
| AD4 | As an admin, I can view platform-wide analytics (GMV, active users, order volume) | Must |
| AD5 | As an admin, I can manage payouts to suppliers/wholesalers | Must |
| AD6 | As an admin, I can configure BNPL credit rules per retailer/tier | Should |

---

## 4. Functional Requirements

### 4.1 Authentication
- Phone number + OTP based login (primary, since target users may not have email)
- Role-based access control (Retailer / Supplier / Wholesaler / Agent / Admin)
- Basic KYC for suppliers/wholesalers (business registration info)

### 4.2 Catalog & Search
- Multi-level categorization (category > subcategory > brand > product)
- Search with autocomplete and filters (price range, brand, in-stock only)
- Multiple sellers can list the same product (price comparison)

### 4.3 Ordering & Cart
- Cart supports multiple suppliers in a single checkout (split into sub-orders per supplier)
- Minimum order quantity (MOQ) support per product/supplier
- Order confirmation and edit window before supplier processes

### 4.4 Payments
- Cash on delivery
- Card payment (via payment gateway)
- Mobile wallet integration
- BNPL via 3rd-party fintech partner (retailer credit line, repayment tracking)

### 4.5 Order Fulfillment & Logistics
- Order status lifecycle: `Placed → Confirmed → Packed → Out for Delivery → Delivered → Completed` (+ `Cancelled`/`Returned`)
- Delivery assignment (to wholesaler's own fleet or 3rd-party logistics partner)
- Real-time order tracking for retailer

### 4.6 Notifications
- Push notifications (order status, promotions, payment reminders)
- SMS fallback for critical updates (OTP, order confirmation) given target market connectivity

### 4.7 Analytics & Reporting
- Supplier-facing: sales trends, top SKUs, demand heatmap by region
- Admin-facing: GMV, active retailers/suppliers, order fulfillment rate, churn

### 4.8 Reviews & Ratings
- Retailer can rate supplier/product/delivery post-order

---

## 5. Non-Functional Requirements

| Category | Requirement |
|---|---|
| Performance | App cold start < 3s; catalog search results < 1s |
| Scalability | Support 100K+ retailers, 10K+ suppliers/wholesalers at scale |
| Availability | 99.5% uptime for core ordering flow |
| Localization | Support Arabic + English (RTL layout support) given regional target market |
| Offline resilience | Cart/browsing should gracefully degrade on poor connectivity (common in target segment) |
| Security | PCI-DSS compliance for payment handling; encrypted storage of KYC/financial data |
| Compliance | Local data residency and financial services regulations for BNPL |

---

## 6. Success Metrics (KPIs)

- **GMV** (Gross Merchandise Value) processed monthly
- **Retailer retention** (% placing repeat orders within 30 days)
- **Order fulfillment rate** (% delivered without cancellation/dispute)
- **Average order value (AOV)**
- **Supplier/wholesaler activation rate**
- **BNPL adoption rate & repayment default rate**
- **Time-to-first-order** for newly onboarded retailers

---

## 7. Release Plan (Phased)

### Phase 1 — MVP (Core Loop)
- Retailer app: browse, order, track (single-city, limited supplier set)
- Supplier dashboard: catalog + order management
- Cash on delivery only
- Manual/semi-manual delivery coordination

### Phase 2 — Scale & Payments
- Card + wallet payment integration
- Multi-city expansion
- Admin panel with analytics
- Sales agent app for field onboarding

### Phase 3 — Financial Services & Optimization
- BNPL integration
- Wholesaler ERP features (inventory management)
- Promotions engine, personalized recommendations
- Advanced analytics/demand forecasting

---

## 8. Open Questions
- Which markets/cities to launch in first?
- Build BNPL in-house or partner with existing fintech from day one?
- Will the platform hold inventory (asset-heavy) or remain purely a matching layer (asset-light, like Cartona)?
- Delivery: in-house fleet vs. 3rd-party logistics vs. wholesaler-managed delivery?
