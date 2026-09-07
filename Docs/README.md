# Wasla Docs — Index

Source-of-truth specs. All 20 docs live here (also mirrored in Notion Docs Library). `Docs/` in this repo is canonical for engineering — Notion is the readable hub.

## Product
- [PRD.md](PRD.md) — requirements, personas (R/S/A/AD), functional & non-functional, release plan
- [Business_Plan.md](Business_Plan.md) — market, model, unit economics, roadmap
- [Project_Plan_Timeline.md](Project_Plan_Timeline.md) — Phase 0→3 sprints, milestones, risks
- [RACI_Matrix.md](RACI_Matrix.md) — ownership across workstreams

## Design
- [Design_System.md](Design_System.md) — tokens, components, a11y, RTL rules
- [Screens_Repository.md](Screens_Repository.md) — full screen catalog across 4 apps
- [Wireframe_Specifications.md](Wireframe_Specifications.md) — layout specs for hi-fi hand-off

## Technical
- [System_Design_Architecture.md](System_Design_Architecture.md) — modular monolith, stack, order flow, repo structure
- [ERD.md](ERD.md) — entities & relationships (Mermaid)
- [Data_Dictionary.md](Data_Dictionary.md) — field-level types, constraints, enums, validation rules
- [API_Contract.md](API_Contract.md) — `POST /api/v1/auth/otp/*` … `GET /analytics/*`, envelopes, errors, idempotency, rate limits
- [Test_Plan.md](Test_Plan.md) — levels, coverage matrix, non-functional, release checklist
- [DevOps_Infra_Runbook.md](DevOps_Infra_Runbook.md) — envs, deploy, rollback, monitoring, backup/DR, scaling
- [Security_Compliance.md](Security_Compliance.md) — OWASP, PCI-DSS, encryption, RBAC
- [Third_Party_Integration_Specs.md](Third_Party_Integration_Specs.md) — SMS, PSP, BNPL, Maps, Logistics
- [Event_Tracking_Analytics_Spec.md](Event_Tracking_Analytics_Spec.md) — events, KPIs, dashboards

## Legal / Ops
- [Terms_of_Service_Privacy_Policy.md](Terms_of_Service_Privacy_Policy.md) — draft ToS + Privacy (pending legal review)
- [Supplier_Wholesaler_Agreement_Template.md](Supplier_Wholesaler_Agreement_Template.md) — seller onboarding contract template
- [Customer_Support_Playbook.md](Customer_Support_Playbook.md) — triage, SLAs, scripts
- [Field_Sales_Agent_Playbook.md](Field_Sales_Agent_Playbook.md) — field ops, onboarding flow

## How to use
- Engineering: treat `Data_Dictionary.md` + `API_Contract.md` + `ERD.md` as the backend contract. Any schema change → update those docs in the same PR.
- Design: `Design_System.md` + `Wireframe_Specifications.md` + `Screens_Repository.md` are the hand-off pack for Figma.
- Planning: `Project_Plan_Timeline.md` drives GitHub Milestones & Project sprints.
