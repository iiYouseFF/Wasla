# Third-Party Integration Specs
## B2B FMCG Marketplace App

**Version:** 1.0
**Status:** Draft — specific vendors to be confirmed; this defines the integration contract shape for each category so backend work can start once a vendor is picked

---

## 1. Payment Gateway (PSP)

**Candidates to evaluate:** Stripe, Paymob, Fawry, PayTabs (evaluate based on target country coverage and local payment method support — card acceptance alone is often insufficient in target markets; wallet support matters)

**Integration points:**
- Create payment session/intent on order checkout
- Webhook listener for payment status updates (`completed`, `failed`, `refunded`)
- Refund API call from Admin panel dispute resolution flow
- Signature verification on all inbound webhooks (reject unsigned/invalid payloads)

**Data exchanged:**
- Outbound: order ID, amount, currency, retailer reference (never raw card data — use PSP-hosted checkout or tokenization to stay out of PCI scope)
- Inbound: transaction ID, status, amount, timestamp

**Failure handling:** if PSP webhook doesn't arrive within a defined SLA window, implement a polling fallback (status check API call) to avoid orders stuck in `pending` indefinitely.

---

## 2. BNPL / Fintech Partner

**Candidates to evaluate:** local/regional embedded finance providers (evaluate licensing status in target market — this is the highest-risk integration from a compliance standpoint, see Terms of Service doc's jurisdiction flags)

**Integration points:**
- Retailer credit eligibility check (real-time API call, likely during onboarding or at checkout)
- Draw request when a BNPL order is placed
- Repayment webhook/sync (partner notifies platform of repayment status)
- Credit limit update sync (partner may periodically re-score retailers)

**Data exchanged:**
- Outbound: retailer identity/KYC reference, order amount, order history (may be required for underwriting)
- Inbound: approved credit limit, draw confirmation, repayment status, default/delinquency flags

**Design note:** Keep the internal `BNPLAccount`/`BNPLTransaction` tables (see Data Dictionary) as a **synced mirror** of the partner's system of record, not the source of truth — this avoids double-bookkeeping disputes if the partner's numbers differ.

---

## 3. SMS / OTP Provider

**Candidates to evaluate:** Twilio, Vonage, or a regional SMS aggregator with better local delivery rates (important — international SMS providers often have poor deliverability/cost in some target markets; check local aggregator options)

**Integration points:**
- Send OTP on registration/login
- Send critical fallback notifications (order confirmation, payment reminders) when push notification fails or app is uninstalled

**Data exchanged:**
- Outbound: phone number, message template ID or raw text
- Inbound: delivery status webhook (optional, for monitoring deliverability)

**Rate limiting:** enforce server-side limits (see API Contract §17) to prevent OTP abuse/cost overruns.

---

## 4. Push Notifications

**Vendor:** Firebase Cloud Messaging (FCM) — recommended default given React Native compatibility and no cost at this scale

**Integration points:**
- Device token registration on app login
- Send notification on order status change, promotion publish, payment reminder
- Handle token refresh/invalidation

**Data exchanged:**
- Outbound: device token, notification payload (title, body, deep-link data)

---

## 5. Maps & Geolocation

**Candidates to evaluate:** Google Maps Platform (most common, good regional coverage), or a regional alternative if cost becomes a concern at scale

**Integration points:**
- Address autocomplete during retailer/agent onboarding
- Reverse geocoding (lat/lng → address) for location pinning
- Live map rendering for delivery tracking screen
- Distance/ETA calculation for delivery assignment logic

**Data exchanged:**
- Outbound: search query or coordinates
- Inbound: address results, place IDs, route/distance data

**Cost note:** Maps APIs are typically billed per request — cache geocoding results where possible (addresses don't change often) to control cost at scale.

---

## 6. Delivery / Logistics Partner (if using 3rd-party, not wholesaler-owned fleet)

**Candidates to evaluate:** regional last-mile logistics providers (varies heavily by market — this should be scoped per launch city)

**Integration points:**
- Delivery order creation (handoff from platform once sub-order is `packed`)
- Real-time tracking webhook/polling for status updates
- Proof-of-delivery capture (photo/signature) sync back to platform
- Failed delivery/return-to-sender handling

**Data exchanged:**
- Outbound: pickup address (seller warehouse), drop-off address (retailer), package details, contact info
- Inbound: tracking status, driver info, ETA, proof-of-delivery

**Design note:** abstract this behind an internal `DeliveryProvider` interface in the Delivery module (see System Design doc) so switching or adding logistics partners per city doesn't require touching core order logic.

---

## 7. Object Storage / CDN

**Vendor:** AWS S3 + CloudFront, or GCP Cloud Storage + Cloud CDN (pick based on chosen cloud provider — see System Design open questions)

**Integration points:**
- Product image upload (Supplier catalog management)
- KYC document upload (Supplier/Wholesaler onboarding)
- Serve optimized/resized images to mobile clients

**Security note:** KYC documents must be stored in a private bucket with signed-URL access only, never publicly readable — separate from product images which can be public/CDN-cached.

---

## 8. Analytics / Crash Reporting

**Vendors:** Sentry (crash/error reporting), a product analytics tool (e.g., Mixpanel, Amplitude, or PostHog for self-hosted/cost control) for the event tracking spec (see separate doc)

**Integration points:**
- SDK embedded in mobile apps and web dashboards
- Event dispatch on key user actions
- Error/crash auto-reporting with breadcrumb context

---

## 9. Integration Readiness Checklist (Per Vendor)

Before backend integration work starts on any vendor above, confirm:
- [ ] Vendor selected and contract/pricing confirmed
- [ ] Sandbox/test credentials obtained
- [ ] Rate limits and pricing tiers understood
- [ ] Webhook endpoint security (signature verification) requirements documented
- [ ] Data residency/compliance requirements checked against vendor's infrastructure locations
- [ ] Fallback/degradation behavior defined if vendor API is down

---

## 10. Open Items
- Finalize PSP and BNPL partner selection — these are the two highest-priority vendor decisions since they block payment and financing module development
- Confirm launch city to finalize logistics partner options (this is city-specific, not a single global choice)
- Confirm cloud provider (AWS vs GCP) to lock in object storage and infra vendor choices consistently
