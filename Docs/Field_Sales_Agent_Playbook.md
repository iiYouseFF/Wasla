# Field Sales Agent Playbook
## B2B FMCG Marketplace App

**Version:** 1.0
**Status:** Draft

---

## 1. Purpose

Field sales agents are the primary retailer acquisition and trust-building channel for this platform — traditional trade retailers typically don't self-onboard onto a new B2B app without a human introducing it. This playbook defines the agent's role, onboarding script framework, incentive structure, and performance expectations.

---

## 2. Role Overview

Field agents are responsible for:
- Identifying and visiting target retailers (shops, cafes, restaurants) in their assigned region
- Explaining the platform's value proposition and completing on-the-spot registration
- Assisting with first order placement to drive early habit formation
- Ongoing relationship management with an assigned retailer portfolio
- Reporting market feedback (competitor activity, pricing issues, product gaps) back to the platform

---

## 3. Retailer Onboarding Process

### Step 1: Identify prospects
- Use assigned territory/route list (prioritize retailers with high foot traffic or existing informal wholesaler relationships as early adopters)

### Step 2: Introduction & pitch
Key talking points (adapt to local language/context):
- Save time: order anytime, no more waiting for wholesaler visits or phone calls
- Better prices: compare multiple suppliers in one app
- Reliable delivery with tracking
- Access to credit (BNPL) once eligible — increases purchasing power without upfront cash

### Step 3: Registration
- Capture business info, phone number, verify with OTP on the spot (agent app)
- Pin exact shop location (critical for delivery accuracy)
- Explain how ordering and payment works in simple terms

### Step 4: First order assistance
- Walk the retailer through browsing and placing their first order together, or place it on their behalf via the agent app's assisted ordering flow
- First-order incentives (discount/promo) should be used here to drive commitment — coordinate with Marketing/Promotions

### Step 5: Follow-up
- Schedule a follow-up visit within the first week to confirm the first order arrived correctly and address any friction
- Log visit notes in the agent app for handoff continuity if the retailer's assigned agent changes later

---

## 4. Ongoing Relationship Management

- Regular visit cadence per retailer (e.g., bi-weekly check-ins, adjust based on order frequency/value)
- Monitor retailer order activity via the agent app dashboard — proactively reach out if a previously active retailer goes quiet (churn risk signal)
- Educate retailers on new features as they launch (BNPL, promotions) rather than assuming self-discovery
- Collect and relay retailer feedback/complaints — agents are often the first to hear about problems before they reach formal support channels

---

## 5. Incentive Structure (Framework — Finalize with Leadership/Finance)

| Metric | Incentive Type |
|---|---|
| New retailer onboarded | Flat bonus per activated account (define "activated" — e.g., completed first order, not just registered) |
| Retailer retention (repeat orders within 30/60 days) | Recurring bonus tied to portfolio retention rate, not just raw signups — avoids incentivizing low-quality signups |
| Order volume driven | Small commission on orders placed/assisted by the agent, if using assisted ordering |
| Visit compliance | Base component tied to completing scheduled visit cadence |

**Design principle:** weight incentives toward retention and order activity, not just raw sign-up count, to avoid agents onboarding retailers who never actually transact.

---

## 6. Performance Metrics (Agent Dashboard)

- Number of retailers onboarded (this period)
- Retailer activation rate (% of onboarded retailers who placed a first order within 7 days)
- Retailer 30-day retention rate (of the agent's portfolio)
- Visit compliance rate (scheduled vs. completed)
- Average order value of assisted orders

---

## 7. Territory & Route Management

- Territories assigned by region/city to avoid overlap between agents
- Route planning should minimize travel time between visits — consider integrating simple route optimization once agent count scales
- Manager oversight: team leads review agent performance weekly, redistribute territory as needed for underperforming/overloaded regions

---

## 8. Training Requirements for New Agents

- Platform product walkthrough (all core retailer-facing features)
- Sales/pitch training with role-play scenarios
- Basic troubleshooting (common app issues agents should be able to resolve on the spot vs. escalate to support)
- Code of conduct: no misrepresenting pricing/terms, no accepting cash on behalf of the platform outside defined COD handling procedures (if agents are ever involved in cash handling — clarify this explicitly to avoid fraud risk)

---

## 9. Escalation Guidance

Agents should escalate (not attempt to resolve themselves):
- Any payment/BNPL disputes → route to Support (see Customer Support Playbook)
- Any suspected fraud (retailer or competing agent behavior) → route to Trust & Safety/Management immediately
- Technical app bugs blocking onboarding → route to Support/Engineering, don't guess workarounds that could cause data issues

---

## 10. Open Items
- Finalize incentive/commission structure with Finance before hiring first agent cohort
- Define "activated retailer" threshold precisely (first order? first N orders? minimum order value?) since it drives incentive payouts
- Decide agent employment structure (in-house employees vs. contracted/gig model) — affects incentive structure design and compliance obligations (labor law considerations, flag for legal counsel)
