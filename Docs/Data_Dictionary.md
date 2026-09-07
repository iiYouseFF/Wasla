# Data Dictionary
## B2B FMCG Marketplace App

**Version:** 1.0
**Status:** Draft
**Purpose:** Field-level specification for every entity in the ERD — data types, constraints, validation rules, and enums. This is the source of truth for backend schema implementation and frontend form validation.

---

## 1. User

| Field | Type | Constraints | Notes |
|---|---|---|---|
| `user_id` | UUID | PK, not null | Generated server-side |
| `phone_number` | string | unique, not null, E.164 format | Primary login identifier |
| `email` | string | nullable, unique if present | Optional |
| `password_hash` | string | nullable | Only if password login is added later |
| `role` | enum | not null | `retailer`, `supplier`, `wholesaler`, `sales_agent`, `admin` |
| `status` | enum | not null, default `pending_verification` | `active`, `suspended`, `pending_verification` |
| `created_at` | timestamp | not null, auto | |
| `updated_at` | timestamp | not null, auto | |

---

## 2. Retailer

| Field | Type | Constraints | Notes |
|---|---|---|---|
| `retailer_id` | UUID | PK, FK → User | |
| `business_name` | string | not null, max 150 chars | |
| `business_type` | enum | not null | `grocery`, `mini_market`, `cafe`, `restaurant`, `kiosk`, `other` |
| `address` | string | not null, max 255 chars | |
| `city_id` | UUID | FK → City, not null | |
| `location_lat` | decimal(9,6) | not null | |
| `location_lng` | decimal(9,6) | not null | |
| `credit_tier` | enum | default `tier_0` | `tier_0` (no credit) through `tier_3` (highest limit) |
| `assigned_agent_id` | UUID | FK → SalesAgent, nullable | |

---

## 3. Supplier

| Field | Type | Constraints | Notes |
|---|---|---|---|
| `supplier_id` | UUID | PK, FK → User | |
| `company_name` | string | not null, max 150 | |
| `business_registration_no` | string | not null, unique | Validated against local business registry format |
| `kyc_status` | enum | default `pending` | `pending`, `verified`, `rejected` |
| `city_id` | UUID | FK → City, not null | |
| `payout_account_info` | JSON | not null once verified | Bank/wallet details, encrypted at rest |

---

## 4. Wholesaler

Same structure as Supplier, plus:

| Field | Type | Constraints | Notes |
|---|---|---|---|
| `warehouse_address` | string | not null | |

---

## 5. SalesAgent

| Field | Type | Constraints | Notes |
|---|---|---|---|
| `agent_id` | UUID | PK, FK → User | |
| `assigned_region_id` | UUID | FK → City, not null | |
| `manager_id` | UUID | FK → SalesAgent, nullable | Self-referencing |

---

## 6. Category

| Field | Type | Constraints | Notes |
|---|---|---|---|
| `category_id` | UUID | PK | |
| `name` | string | not null, max 100 | |
| `parent_category_id` | UUID | FK → Category, nullable | Null = top-level category |

---

## 7. Product

| Field | Type | Constraints | Notes |
|---|---|---|---|
| `product_id` | UUID | PK | |
| `name` | string | not null, max 200 | |
| `brand` | string | not null, max 100 | |
| `category_id` | UUID | FK → Category, not null | |
| `unit_of_measure` | enum | not null | `piece`, `pack`, `carton`, `kg`, `liter`, `box` |
| `description` | text | nullable, max 2000 chars | |
| `image_url` | string | not null | Must be valid HTTPS URL to object storage |

---

## 8. Listing

| Field | Type | Constraints | Notes |
|---|---|---|---|
| `listing_id` | UUID | PK | |
| `product_id` | UUID | FK → Product, not null | |
| `seller_id` | UUID | not null | References Supplier or Wholesaler per `seller_type` |
| `seller_type` | enum | not null | `supplier`, `wholesaler` |
| `price` | decimal(10,2) | not null, > 0 | Currency assumed platform-wide single currency at MVP |
| `min_order_qty` | integer | not null, default 1, >= 1 | |
| `is_active` | boolean | not null, default true | |
| `created_at` / `updated_at` | timestamp | not null, auto | |

---

## 9. Inventory

| Field | Type | Constraints | Notes |
|---|---|---|---|
| `inventory_id` | UUID | PK | |
| `listing_id` | UUID | FK → Listing, not null | |
| `warehouse_location` | string | not null | |
| `quantity_available` | integer | not null, >= 0 | Must never go negative — enforce at DB/application layer with row locking on order creation |
| `last_updated_at` | timestamp | not null, auto | |

---

## 10. Cart / CartItem

| Field | Type | Constraints | Notes |
|---|---|---|---|
| `cart_id` | UUID | PK | |
| `retailer_id` | UUID | FK → Retailer, not null, unique (1 active cart per retailer) | |
| `cart_item_id` | UUID | PK | |
| `cart_id` | UUID | FK → Cart, not null | |
| `listing_id` | UUID | FK → Listing, not null | |
| `quantity` | integer | not null, >= 1 | Validated against `min_order_qty` at checkout, not at add-to-cart |

---

## 11. Order

| Field | Type | Constraints | Notes |
|---|---|---|---|
| `order_id` | UUID | PK | |
| `retailer_id` | UUID | FK → Retailer, not null | |
| `order_status` | enum | not null, default `placed` | `placed`, `confirmed`, `partially_fulfilled`, `completed`, `cancelled` |
| `total_amount` | decimal(10,2) | not null, >= 0 | Sum of all sub-order subtotals |
| `payment_method` | enum | not null | `cod`, `card`, `wallet`, `bnpl` |
| `created_at` / `updated_at` | timestamp | not null, auto | |

**Derived rule:** `order_status` is computed/rolled up from constituent `sub_order.status` values (e.g., all sub-orders `completed` → order `completed`; any `cancelled` and rest `completed` → order `partially_fulfilled`).

---

## 12. SubOrder

| Field | Type | Constraints | Notes |
|---|---|---|---|
| `sub_order_id` | UUID | PK | |
| `order_id` | UUID | FK → Order, not null | |
| `seller_id` | UUID | not null | |
| `seller_type` | enum | not null | `supplier`, `wholesaler` |
| `status` | enum | not null, default `pending` | `pending`, `confirmed`, `packed`, `out_for_delivery`, `delivered`, `completed`, `cancelled`, `returned` |
| `subtotal_amount` | decimal(10,2) | not null, >= 0 | |

**Valid status transitions (enforced server-side):**
`pending → confirmed → packed → out_for_delivery → delivered → completed`
`pending/confirmed → cancelled` (before packing only)
`delivered → returned` (within return window, if applicable)

---

## 13. OrderItem

| Field | Type | Constraints | Notes |
|---|---|---|---|
| `order_item_id` | UUID | PK | |
| `sub_order_id` | UUID | FK → SubOrder, not null | |
| `listing_id` | UUID | FK → Listing, not null | |
| `quantity` | integer | not null, >= 1 | |
| `unit_price` | decimal(10,2) | not null | Snapshot of price at time of order (immutable even if listing price changes later) |
| `line_total` | decimal(10,2) | not null | `quantity * unit_price` |

---

## 14. Payment

| Field | Type | Constraints | Notes |
|---|---|---|---|
| `payment_id` | UUID | PK | |
| `order_id` | UUID | FK → Order, not null | |
| `amount` | decimal(10,2) | not null, > 0 | |
| `method` | enum | not null | `cod`, `card`, `wallet`, `bnpl` |
| `status` | enum | not null, default `pending` | `pending`, `completed`, `failed`, `refunded` |
| `transaction_reference` | string | nullable | External PSP reference |
| `paid_at` | timestamp | nullable | Set when status → `completed` |

---

## 15. BNPLAccount

| Field | Type | Constraints | Notes |
|---|---|---|---|
| `bnpl_account_id` | UUID | PK | |
| `retailer_id` | UUID | FK → Retailer, unique | |
| `credit_limit` | decimal(10,2) | not null, >= 0 | Set by admin/credit engine based on `credit_tier` |
| `available_credit` | decimal(10,2) | not null, >= 0, <= credit_limit | |
| `status` | enum | not null, default `active` | `active`, `suspended`, `defaulted` |

---

## 16. BNPLTransaction

| Field | Type | Constraints | Notes |
|---|---|---|---|
| `bnpl_transaction_id` | UUID | PK | |
| `bnpl_account_id` | UUID | FK → BNPLAccount, not null | |
| `order_id` | UUID | FK → Order, nullable | Null for standalone repayments |
| `type` | enum | not null | `draw`, `repayment` |
| `amount` | decimal(10,2) | not null, > 0 | |
| `due_date` | date | not null for `draw` type | |
| `paid_date` | date | nullable | |
| `status` | enum | not null, default `pending` | `pending`, `paid`, `overdue` |

---

## 17. Delivery

| Field | Type | Constraints | Notes |
|---|---|---|---|
| `delivery_id` | UUID | PK | |
| `sub_order_id` | UUID | FK → SubOrder, unique | |
| `delivery_partner_type` | enum | not null | `wholesaler_fleet`, `third_party` |
| `driver_name` | string | nullable | |
| `tracking_status` | string | nullable | Free-text or synced enum from 3rd-party API |
| `estimated_delivery_time` | timestamp | nullable | |
| `actual_delivery_time` | timestamp | nullable | |

---

## 18. Review

| Field | Type | Constraints | Notes |
|---|---|---|---|
| `review_id` | UUID | PK | |
| `retailer_id` | UUID | FK → Retailer, not null | |
| `sub_order_id` | UUID | FK → SubOrder, not null, unique per retailer | One review per sub-order |
| `rating` | integer | not null, 1-5 | |
| `comment` | text | nullable, max 1000 chars | |
| `created_at` | timestamp | not null, auto | |

---

## 19. Promotion

| Field | Type | Constraints | Notes |
|---|---|---|---|
| `promotion_id` | UUID | PK | |
| `seller_id` | UUID | not null | |
| `listing_id` | UUID | FK → Listing, nullable | Null = category-wide promotion |
| `discount_type` | enum | not null | `percentage`, `fixed_amount` |
| `discount_value` | decimal(10,2) | not null, > 0; if `percentage`, <= 100 | |
| `start_date` / `end_date` | date | not null; `end_date` >= `start_date` | |

---

## 20. Payout

| Field | Type | Constraints | Notes |
|---|---|---|---|
| `payout_id` | UUID | PK | |
| `seller_id` | UUID | not null | |
| `amount` | decimal(10,2) | not null, > 0 | |
| `period_start` / `period_end` | date | not null | |
| `status` | enum | not null, default `pending` | `pending`, `paid` |
| `paid_at` | timestamp | nullable | |

---

## 21. Notification

| Field | Type | Constraints | Notes |
|---|---|---|---|
| `notification_id` | UUID | PK | |
| `user_id` | UUID | FK → User, not null | |
| `type` | enum | not null | `order_update`, `promotion`, `payment_reminder` |
| `message` | string | not null, max 500 | |
| `is_read` | boolean | not null, default false | |
| `created_at` | timestamp | not null, auto | |

---

## 22. City

| Field | Type | Constraints | Notes |
|---|---|---|---|
| `city_id` | UUID | PK | |
| `name` | string | not null, unique within country | |
| `country` | string | not null | |

---

## 23. Cross-Entity Validation Rules

- An `Order` cannot be created if any `Listing` in the cart has `quantity_available` less than the requested quantity at the time of checkout (checked with row-level lock to avoid race conditions).
- `CartItem.quantity` must satisfy `Listing.min_order_qty` before checkout is allowed (validated at checkout, not at add-to-cart, so users can build up quantity incrementally).
- `BNPLTransaction` of type `draw` cannot exceed `BNPLAccount.available_credit` at the time of order placement.
- `Review` can only be created if the related `SubOrder.status` is `delivered` or `completed`.
- `Promotion.end_date` must not be earlier than `start_date`; overlapping promotions on the same `listing_id` should be flagged for seller review (not hard-blocked at MVP).
