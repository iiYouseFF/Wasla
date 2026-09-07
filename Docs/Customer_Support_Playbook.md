# Customer Support Playbook
## B2B FMCG Marketplace App

**Version:** 1.0
**Status:** Draft

---

## 1. Purpose

Defines how the support team handles Retailer and Supplier/Wholesaler inquiries, escalation paths, and SLAs, so support scales consistently as the platform grows beyond founder-handled support.

---

## 2. Support Channels

| Channel | Use Case | Priority |
|---|---|---|
| In-app chat/help center | Primary channel for retailers and sellers | Primary |
| Phone support | Order emergencies, retailers with low digital literacy | Secondary, high-touch |
| WhatsApp/SMS (if applicable regionally) | Common preferred channel in target market — evaluate as a support channel, not just notifications | Consider for MVP+ |
| Email | Supplier/Wholesaler account-level or contractual issues | Lower priority for time-sensitive issues |

---

## 3. Issue Categories & Routing

| Category | Examples | Routed To |
|---|---|---|
| Order issues | Wrong item, missing item, late delivery, damaged goods | Support agent → Seller (if fulfillment fault) |
| Payment issues | Failed payment, refund request, double charge | Support agent → Finance/Payments team |
| BNPL issues | Credit limit question, repayment dispute, account suspension appeal | Support agent → BNPL/Finance team |
| Account issues | Login problems, KYC rejection appeal, profile update help | Support agent → Ops/Admin |
| Catalog/listing issues (seller-reported) | Incorrect product info, competitor pricing complaint | Support agent → Catalog/Ops team |
| Technical/app bugs | Crashes, UI issues | Support agent → Engineering (via bug tracker) |
| Fraud/abuse reports | Fake orders, counterfeit goods, harassment | Support agent → Trust & Safety/Admin (urgent) |

---

## 4. Service Level Agreements (SLA)

| Priority | Definition | First Response Target | Resolution Target |
|---|---|---|---|
| **Urgent** | Payment/financial error, fraud, safety issue | 30 minutes | 4 hours |
| **High** | Order not delivered, significant delay, account locked out | 2 hours | 24 hours |
| **Medium** | General order questions, non-blocking account issues | 8 hours (same business day) | 48 hours |
| **Low** | Feature requests, general feedback | 24 hours | Best effort |

---

## 5. Standard Response Framework

1. **Acknowledge** — confirm receipt and understanding of the issue
2. **Verify** — confirm identity/order details before taking action (never act on account/order changes without verification)
3. **Investigate** — check order/payment/account status in admin tools
4. **Resolve or Escalate** — resolve directly if within agent authority; escalate per routing table above if not
5. **Confirm** — inform the user of the resolution and confirm they're satisfied
6. **Log** — record resolution in support ticketing system for pattern tracking

---

## 6. Agent Authority Levels (Define Per Team Size/Maturity)

| Action | Frontline Agent | Team Lead | Finance/Ops |
|---|---|---|---|
| Answer general questions | ✅ | ✅ | ✅ |
| Issue refund under a defined threshold | ✅ (up to defined limit) | ✅ | ✅ |
| Issue refund above threshold | ❌ | ✅ | ✅ |
| Suspend/reinstate account | ❌ | ✅ (with review) | ✅ |
| Adjust BNPL credit limit | ❌ | ❌ | ✅ only |
| Approve seller payout exception | ❌ | ❌ | ✅ only |

*(Fill in specific refund threshold once finance defines it.)*

---

## 7. Common Scenarios & Guidance

### 7.1 Retailer reports item not received
- Check delivery tracking status in admin tools
- If marked delivered but retailer disputes: request photo evidence if available, contact delivery partner/seller for confirmation
- If genuinely lost: process replacement or refund per policy, and log a delivery-partner performance flag

### 7.2 Retailer disputes BNPL charge
- Verify the order and BNPL transaction record match
- Do not unilaterally waive BNPL fees/interest — escalate to Finance/BNPL team, as this may involve the external fintech partner's terms

### 7.3 Supplier complains about a competitor's pricing
- This is a business/product policy question (marketplace price transparency is often intentional), not a support fix — escalate to Product/Ops team, do not promise pricing changes

### 7.4 Suspected fraudulent order (fake retailer account ordering to resell at loss, etc.)
- Escalate immediately to Trust & Safety/Admin — do not resolve as a standard order issue; may require account suspension pending investigation

---

## 8. Tools

- Ticketing system (e.g., Zendesk, Freshdesk, or a simpler tool at MVP stage)
- Read access to Admin panel (order, payment, account status) — support agents should not have write access beyond their authority level (see §6)
- Internal knowledge base for FAQs and scripted responses to common questions

---

## 9. Quality & Feedback Loop

- Track: first response time, resolution time, customer satisfaction (CSAT) score per ticket, ticket volume by category
- Weekly review of ticket categories to identify recurring product/ops issues that should be escalated as feature requests or bug fixes rather than repeatedly handled manually
- Feed common/repeated issues back into the Event Tracking spec and Product team as signals of friction points

---

## 10. Open Items
- Define exact refund authority thresholds with Finance
- Decide on ticketing tool and initial team size/shift coverage (support hours — 24/7 not necessary at MVP, but should be clearly communicated to users)
- Build out the in-app help center content (FAQ articles) before or alongside MVP launch
