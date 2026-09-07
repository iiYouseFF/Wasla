# Project Plan & Timeline
## B2B FMCG Marketplace App

**Version:** 1.0
**Status:** Draft — durations are placeholder estimates; adjust once team size/velocity is known

---

## 1. Overview

This plan sequences the work across all workstreams (product, design, engineering, ops, legal) from where you are now (docs complete) through MVP launch and early scale phases, aligned with the phased roadmap in the Business Plan and PRD.

---

## 2. Phase 0 — Pre-Development Foundation (Current Phase)

**Goal:** All planning docs complete, key vendor/legal decisions made, team assembled.

| Workstream | Task | Owner |
|---|---|---|
| Product | PRD, ERD, Business Plan ✅ | Done |
| Design | Screens repository, wireframe specs, design system ✅ | Done |
| Technical | System design, API contract, data dictionary, test plan, security doc, DevOps runbook, integration specs, analytics spec ✅ | Done |
| Legal/Ops | ToS/Privacy Policy draft, Supplier agreement template, support/agent playbooks ✅ | Done |
| **Remaining before dev starts** | Finalize cloud provider, PSP, BNPL partner, logistics partner (per city), launch city selection | Founders/Leadership |
| **Remaining before dev starts** | Legal review of ToS, Privacy Policy, Supplier Agreement, and BNPL compliance structure | External legal counsel |
| **Remaining before dev starts** | High-fidelity UI design (Figma) based on wireframe specs + design system | Designer |
| **Remaining before dev starts** | Hire/assign core engineering team (backend, mobile, web) | Founders/Leadership |

**Exit criteria:** vendor decisions locked, legal docs reviewed, high-fidelity designs ready, team in place.

---

## 3. Phase 1 — MVP Build (Estimated 10-14 weeks, adjust per team size)

**Goal:** Core ordering loop live in a single pilot city, COD payment only.

| Sprint (2-week) | Focus |
|---|---|
| Sprint 1-2 | Infra setup (per DevOps runbook), auth module, database schema implementation (per Data Dictionary) |
| Sprint 3-4 | Catalog module (product/listing CRUD), Retailer app: onboarding + browse/search screens |
| Sprint 5-6 | Cart & checkout (COD only), Order module with sub-order splitting logic |
| Sprint 7 | Supplier dashboard: catalog management + order management screens |
| Sprint 8 | Delivery tracking (basic status updates, manual/semi-manual coordination acceptable at MVP) |
| Sprint 9 | Admin panel: account approval, basic order operations |
| Sprint 10 | QA hardening per Test Plan, bug fixing, staging UAT with pilot retailers/suppliers |

**Exit criteria:** core loop (browse → order → fulfill → track) works end-to-end in staging with real pilot users in UAT; Test Plan release checklist passed.

---

## 4. Phase 2 — Pilot Launch & Payments (Estimated 6-8 weeks)

| Sprint | Focus |
|---|---|
| Sprint 11 | Production deploy, soft launch with initial retailer/supplier cohort (agent-driven onboarding) |
| Sprint 12-13 | Card + wallet payment integration (per chosen PSP) |
| Sprint 14 | Sales agent app (if not already needed for pilot onboarding) |
| Sprint 15-16 | Analytics dashboards (supplier + admin), event tracking implementation per Analytics Spec |

**Exit criteria:** pilot city showing repeat order activity; payment methods beyond COD live; basic analytics visibility for leadership.

---

## 5. Phase 3 — Financial Services & Scale Prep (Estimated 8-12 weeks)

| Sprint | Focus |
|---|---|
| Sprint 17-19 | BNPL integration with selected fintech partner (pending licensing/legal clearance) |
| Sprint 20-21 | Promotions engine, seller-side promotion tools |
| Sprint 22-23 | Multi-city readiness (city-aware logistics/inventory logic, if not already built in from Phase 1 data model) |
| Sprint 24 | Performance/load testing ahead of 2nd city launch |

**Exit criteria:** BNPL live and monitored for default rates; platform technically ready for 2nd city expansion.

---

## 6. Ongoing / Parallel Workstreams (Not Sprint-Bound)

- Field agent hiring & training ramps ahead of each city launch
- Customer support team scaling in line with retailer/order volume growth
- Continuous security review (dependency scanning every build, scheduled penetration test before public launch)
- Legal/compliance monitoring, especially around BNPL regulatory status

---

## 7. Key Dependencies & Risks to Timeline

| Dependency | Risk if Delayed |
|---|---|
| BNPL partner/licensing confirmation | Blocks Phase 3 entirely — start this conversation early, in parallel with Phase 1 build, not after |
| Launch city selection | Blocks logistics partner selection and field agent hiring |
| High-fidelity design completion | Blocks frontend sprint start in Phase 1 |
| Cloud/vendor selection | Blocks infra setup in Sprint 1 |

**Recommendation:** Treat BNPL partner selection and legal clearance as a critical path item to start immediately in parallel with Phase 1 engineering work, since it has the longest lead time (partner negotiation + regulatory confirmation) relative to when it's actually needed (Phase 3).

---

## 8. Milestone Summary

| Milestone | Target (relative to Phase 0 completion) |
|---|---|
| Dev environment + infra live | End of Phase 1, Sprint 2 |
| MVP feature-complete in staging | End of Phase 1, Sprint 10 |
| Pilot city soft launch | Start of Phase 2 |
| Card/wallet payments live | Mid Phase 2 |
| First 100 active retailers, 10 suppliers | End of Phase 2 |
| BNPL pilot live | Mid Phase 3 |
| 2nd city launch readiness | End of Phase 3 |

---

## 9. Open Items
- Convert sprint estimates to actual calendar dates once team size and start date are confirmed
- Confirm whether Sales Agent app is needed for Phase 1 (if pilot city is small enough for manual/spreadsheet-based agent coordination initially, this could shift to Phase 2 to reduce Phase 1 scope)
- Re-baseline this plan after Phase 1 sprint 1-2 actuals are known — initial estimates should be treated as directional, not committed
