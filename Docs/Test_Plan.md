# Test Plan & QA Strategy
## B2B FMCG Marketplace App

**Version:** 1.0
**Status:** Draft

---

## 1. Objectives

Ensure the platform's core transactional flows (ordering, payment, fulfillment) are reliable, secure, and function correctly across all four client apps before and after each release, with particular attention to the multi-seller order-splitting logic and financial correctness (payments, BNPL, payouts).

---

## 2. Test Levels

| Level | Scope | Owner |
|---|---|---|
| Unit tests | Individual functions/modules (e.g., order total calculation, BNPL credit check) | Backend/frontend engineers |
| Integration tests | Module-to-module interactions (e.g., Order module → Inventory module stock deduction) | Backend engineers |
| API/contract tests | Every endpoint in the API Contract doc — request/response schema validation | QA + backend engineers |
| End-to-end (E2E) tests | Full user flows across UI (e.g., browse → cart → checkout → order confirmation) | QA (automated + manual) |
| Manual exploratory testing | Edge cases, usability issues not caught by scripted tests | QA |
| Performance/load testing | Catalog search, checkout under concurrent load | QA/DevOps |
| Security testing | Auth, RBAC, payment data handling, injection/XSS | QA + external security review before launch |
| UAT (User Acceptance Testing) | Real retailers/suppliers testing pilot build | Product + pilot users |

---

## 3. Environments

| Environment | Purpose |
|---|---|
| `local` | Developer machines |
| `staging` | Full integration testing, mirrors production config with test data |
| `sandbox` (3rd-party) | Payment gateway/BNPL provider sandbox mode for safe testing |
| `production` | Live environment |

**Rule:** No real payment credentials or production data in `staging`. All 3rd-party integrations (PSP, BNPL, SMS) must have sandbox/test modes wired into `staging`.

---

## 4. Test Coverage by Feature Area

### 4.1 Authentication
- [ ] OTP request succeeds for valid phone number
- [ ] OTP request rate-limited after N attempts
- [ ] OTP verify fails with incorrect/expired code
- [ ] Token refresh flow works after access token expiry
- [ ] Role-based access: retailer cannot access supplier-only endpoints (expect `403`)

### 4.2 Catalog & Search
- [ ] Search returns relevant results for partial/misspelled queries
- [ ] Category filter narrows results correctly
- [ ] Listing price comparison shows all active sellers for a product
- [ ] Inactive listings (`is_active = false`) do not appear in search results
- [ ] Out-of-stock listings show correct status and are not addable to cart

### 4.3 Cart & Checkout
- [ ] Adding item below MOQ is allowed in cart but blocked at checkout with clear messaging
- [ ] Cart correctly groups items by seller
- [ ] Removing all items from a seller group removes that group's section
- [ ] Checkout total matches sum of all group subtotals + any fees
- [ ] Checkout blocked if any item's stock changed to below requested quantity since add-to-cart (re-validate at checkout)

### 4.4 Orders
- [ ] Order creation splits correctly into sub-orders per seller
- [ ] Idempotency key prevents duplicate order creation on double-submit/retry
- [ ] Order status rollup logic correct (e.g., one sub-order cancelled + rest completed → `partially_fulfilled`)
- [ ] Sub-order status transitions follow allowed state machine only (invalid transitions rejected with `422`)
- [ ] Order cancellation only allowed in valid states (e.g., not after `out_for_delivery`)
- [ ] Concurrent order placement against limited stock does not oversell (race condition test — critical)

### 4.5 Payments
- [ ] COD orders skip payment gateway, mark payment `pending` until delivery confirmation
- [ ] Card payment redirects to PSP and correctly updates order status on webhook callback
- [ ] Failed payment does not confirm the order
- [ ] Refund flow correctly reverses payment status and notifies retailer
- [ ] Webhook signature verification rejects tampered/forged callbacks

### 4.6 BNPL
- [ ] Order blocked if `amount > available_credit`
- [ ] Successful BNPL draw reduces `available_credit` correctly
- [ ] Repayment correctly restores `available_credit`
- [ ] Overdue repayment correctly flags account status
- [ ] BNPL account suspension blocks new BNPL draws but not other payment methods

### 4.7 Delivery
- [ ] Tracking screen reflects real-time status updates
- [ ] ETA displays correctly and updates as status changes
- [ ] Delivery completion triggers correct downstream events (notification, review eligibility)

### 4.8 Reviews
- [ ] Review submission only allowed post-delivery
- [ ] Duplicate review per sub-order blocked
- [ ] Rating average recalculates correctly on new review

### 4.9 Promotions
- [ ] Promotion correctly applies discount within date range only
- [ ] Expired promotions no longer apply to price calculation
- [ ] Overlapping promotions handled per defined business rule (flag or last-wins — confirm with product)

### 4.10 Admin Operations
- [ ] Account approval transitions KYC status correctly and notifies applicant
- [ ] Suspended accounts cannot log in or transact
- [ ] Dispute resolution correctly updates order/payment/refund state together (no partial state left inconsistent)

### 4.11 Notifications
- [ ] Push notification delivered on order status change
- [ ] SMS fallback triggers if push token unavailable/failed for critical alerts (OTP, order confirmation)

---

## 5. Non-Functional Testing

| Type | Approach |
|---|---|
| **Performance** | Load test checkout + search endpoints at expected peak concurrency (define target after pilot data) |
| **Security** | OWASP Top 10 checklist review, dependency vulnerability scanning (e.g., `npm audit`/Snyk), penetration test before public launch |
| **Accessibility** | Basic mobile accessibility checks (tap target sizes, contrast ratios, screen reader labels) |
| **Localization** | Verify Arabic RTL layout renders correctly across all screens; verify date/currency formatting per locale |
| **Compatibility** | Test on minimum supported OS versions (define matrix — e.g., Android 8+, iOS 14+) and low-end device performance given target market device profile |
| **Offline/poor connectivity** | Verify graceful degradation and retry behavior on flaky network (common in target market) |

---

## 6. Test Data Strategy

- Maintain a seeded `staging` dataset covering: multiple cities, multiple sellers per product (for price comparison testing), retailers across all credit tiers, at least one BNPL account near its limit, and orders in every possible status for UI/state testing.
- Synthetic data generation script recommended (seed script) rather than manual staging data entry, to keep it reproducible.

---

## 7. Bug Triage & Severity Definitions

| Severity | Definition | Response Target |
|---|---|---|
| **Critical (P0)** | Payment/financial data corruption, security breach, complete outage, overselling stock | Immediate hotfix |
| **High (P1)** | Core flow broken for a subset of users (e.g., checkout fails for BNPL users only) | Fix within 24-48h |
| **Medium (P2)** | Non-blocking functional bug, workaround exists | Fix in next sprint |
| **Low (P3)** | Cosmetic/UI polish issue | Backlog |

---

## 8. Release Testing Checklist (Per Release)

- [ ] All P0/P1 bugs from previous release resolved or explicitly deferred with sign-off
- [ ] Regression suite passed on `staging`
- [ ] API contract tests passed against updated backend
- [ ] Manual smoke test of core flow (browse → cart → checkout → track order) on both iOS and Android
- [ ] Payment/BNPL sandbox transactions verified end-to-end
- [ ] Rollback plan confirmed before production deploy

---

## 9. Tooling Recommendations

| Purpose | Tool |
|---|---|
| Unit/integration testing (backend) | Jest (Node.js/NestJS) |
| API contract testing | Postman/Newman or Jest + Supertest |
| E2E mobile testing | Detox (React Native) |
| E2E web testing | Playwright or Cypress |
| Load testing | k6 or Artillery |
| Bug tracking | Jira or Linear |
| CI integration | Run unit + API contract tests on every PR via GitHub Actions; run E2E suite nightly or pre-release |

---

## 10. Open Items
- Define exact device/OS support matrix based on target market device data
- Define performance/load targets once pilot city order volume assumptions are set
- Confirm whether a formal external security audit/pen test is required before launch (recommended before handling live payment/BNPL data)
