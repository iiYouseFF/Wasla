# Event Tracking & Analytics Spec
## B2B FMCG Marketplace App

**Version:** 1.0
**Status:** Draft

---

## 1. Purpose

Defines what user/product events are tracked, naming conventions, and required properties — separate from the business KPI dashboards in the PRD/Business Plan, this is the granular event layer that feeds product analytics tools (Mixpanel/Amplitude/PostHog) for funnel analysis, retention cohorts, and feature usage.

---

## 2. Naming Convention

- Format: `object_action` in `snake_case` (e.g., `order_placed`, `product_viewed`)
- Events are named from the user's action, not the system's internal process
- Every event includes standard base properties (§3) plus event-specific properties (§4)

---

## 3. Standard Base Properties (attached to every event)

| Property | Type | Description |
|---|---|---|
| `user_id` | string | Authenticated user ID (null if pre-auth) |
| `role` | string | `retailer`, `supplier`, `wholesaler`, `sales_agent`, `admin` |
| `city_id` | string | User's registered city |
| `platform` | string | `ios`, `android`, `web` |
| `app_version` | string | Client app version |
| `timestamp` | ISO 8601 | Event time |
| `session_id` | string | Client session identifier |

---

## 4. Event Catalog

### 4.1 Auth & Onboarding
| Event | Properties | Notes |
|---|---|---|
| `otp_requested` | `phone_number_hash` | Hash, never raw phone number, for privacy |
| `otp_verified` | `success: boolean` | |
| `registration_completed` | `business_type`, `city_id` | Fired once profile setup is complete |
| `onboarding_step_viewed` | `step_name` | For funnel drop-off analysis |

### 4.2 Catalog & Discovery
| Event | Properties | Notes |
|---|---|---|
| `home_viewed` | — | |
| `category_viewed` | `category_id`, `category_name` | |
| `search_performed` | `query`, `result_count` | |
| `search_result_clicked` | `query`, `product_id`, `position` | Position = rank in result list, for search relevance tuning |
| `product_viewed` | `product_id`, `category_id`, `entry_point` (home/search/category) | |
| `seller_comparison_viewed` | `product_id`, `listing_count` | |

### 4.3 Cart & Checkout
| Event | Properties | Notes |
|---|---|---|
| `product_added_to_cart` | `listing_id`, `product_id`, `quantity`, `price` | |
| `product_removed_from_cart` | `listing_id` | |
| `cart_viewed` | `item_count`, `total_value` | |
| `checkout_started` | `cart_total`, `seller_count` | |
| `payment_method_selected` | `method` | |
| `checkout_completed` | `order_id`, `total_amount`, `payment_method`, `sub_order_count` | Core conversion event |
| `checkout_failed` | `reason` (`stock_conflict`, `payment_declined`, `moq_not_met`, etc.) | Critical for funnel diagnosis |

### 4.4 Orders
| Event | Properties | Notes |
|---|---|---|
| `order_viewed` | `order_id` | |
| `order_tracked` | `order_id`, `sub_order_id` | |
| `order_cancelled` | `order_id`, `reason` | |
| `reorder_initiated` | `original_order_id` | |
| `review_submitted` | `sub_order_id`, `rating` | |

### 4.5 BNPL
| Event | Properties | Notes |
|---|---|---|
| `bnpl_account_viewed` | `available_credit` | |
| `bnpl_used_at_checkout` | `order_id`, `amount` | |
| `bnpl_repayment_made` | `amount`, `on_time: boolean` | |
| `bnpl_limit_reached_blocked` | `attempted_amount`, `available_credit` | Signals demand for higher limits — useful for credit product decisions |

### 4.6 Supplier/Wholesaler Dashboard
| Event | Properties | Notes |
|---|---|---|
| `product_listed` | `product_id`, `price` | |
| `listing_updated` | `listing_id`, `field_changed` | |
| `order_status_updated` | `sub_order_id`, `new_status` | |
| `promotion_created` | `promotion_id`, `discount_type`, `discount_value` | |
| `analytics_dashboard_viewed` | `date_range` | |
| `payout_viewed` | — | |

### 4.7 Sales Agent App
| Event | Properties | Notes |
|---|---|---|
| `retailer_onboarded_by_agent` | `retailer_id`, `agent_id` | |
| `visit_checked_in` | `retailer_id`, `location_lat`, `location_lng` | |
| `assisted_order_placed` | `order_id`, `retailer_id` | |

### 4.8 Admin
| Event | Properties | Notes |
|---|---|---|
| `account_approved` | `target_user_id`, `role` | |
| `account_suspended` | `target_user_id`, `reason` | |
| `dispute_resolved` | `order_id`, `resolution_type` | |

---

## 5. Key Funnels to Build in Analytics Tool

1. **Retailer activation funnel:** `registration_completed → product_viewed → product_added_to_cart → checkout_completed`
2. **Retailer retention:** repeat `checkout_completed` events within 7/30-day windows post first order
3. **Supplier activation funnel:** `product_listed → order_status_updated (first order received) → payout_viewed`
4. **BNPL adoption funnel:** `bnpl_account_viewed → bnpl_used_at_checkout → bnpl_repayment_made (on time)`
5. **Search effectiveness:** `search_performed → search_result_clicked` ratio, and `search_performed` with `result_count: 0` rate (unmet demand signal for catalog gaps)

---

## 6. Privacy & Compliance Notes

- Never send raw phone numbers, payment card data, or KYC document content as event properties — hash or omit
- Ensure analytics tool's data residency is compatible with the jurisdiction-specific requirements flagged in the Privacy Policy doc
- Provide a mechanism to exclude a user's events from analytics if they exercise a data deletion right (see Privacy Policy §6)

---

## 7. Implementation Notes

- Instrument events at the point of user action in the client (not inferred server-side after the fact) for accurate `entry_point`/context properties, except for backend-only events (e.g., webhook-triggered status changes) which should be tracked server-side
- Maintain this document as the single source of truth — add new events here before implementing, not after, to avoid naming drift across platforms (iOS/Android/Web)
- Recommend a lightweight internal review (product + eng) before adding new events to avoid uncontrolled event sprawl

---

## 8. Open Items
- Finalize analytics tool choice (Mixpanel vs Amplitude vs PostHog) based on budget and self-hosting/data-residency needs
- Define which events also need to feed the Supplier/Admin-facing analytics dashboards (some overlap with business KPIs in the PRD, but the dashboards likely aggregate from transactional DB directly rather than the event stream — clarify data source per dashboard metric)
