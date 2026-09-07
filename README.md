# Wasla — B2B FMCG Marketplace

> Digitizing Egypt's traditional trade supply chain. Inspired by **Cartona's** asset-light B2B marketplace model.

**Wasla** connects small retailers (corner shops, cafes, restaurants) directly to FMCG suppliers and wholesalers through a mobile-first ordering platform — with embedded financing (BNPL), real-time inventory, and order tracking.

---

## 📦 Repository Purpose

This is the **main monorepo** for Wasla. It hosts:

- **Docs** — source-of-truth product, design, technical, and legal specs (`/Docs`)
- **Apps** — all four client surfaces
  - `apps/retailer-mobile` — Retailer Mobile App (React Native)
  - `apps/supplier-dashboard` — Supplier/Wholesaler Web Dashboard (Next.js)
  - `apps/agent-mobile` — Sales Agent Mobile App (React Native, Android-first)
  - `apps/admin-panel` — Admin Web Panel (Next.js)
- **Packages** — shared code
  - `packages/shared-types` — TypeScript types shared across apps + backend
  - `packages/ui-components` — design system components (tokens from Design System)
- **Infra** — deployment & infra-as-code (see `Wasla-backend` repo for API)

> **Backend API** lives in [`iiYouseFF/Wasla-backend`](https://github.com/iiYouseFF/Wasla-backend) — a NestJS modular monolith (see System Design doc).

---

## 🏗️ Architecture

**Modular monolith → microservices later.** Single deployable backend organized into domain modules (Auth, Catalog, Orders, Inventory, Payments, BNPL, Delivery, Notifications, Analytics). Split only when scale/team-size demands it.

```
Clients (4 apps) → API Gateway → Backend Modules → Data (Postgres, Redis, ES, S3) + External (SMS, PSP, BNPL, Maps, Logistics)
```

Tech stack: React Native + Next.js · NestJS · PostgreSQL · Redis · Elasticsearch (or Postgres FTS for MVP) · S3 · RabbitMQ/SQS · FCM · Twilio · Stripe/regional PSP

See [`Docs/System_Design_Architecture.md`](Docs/System_Design_Architecture.md) and [`Docs/ERD.md`](Docs/ERD.md) for full diagrams.

---

## 📚 Documentation (20 docs — Phase 0 complete)

| Category | Docs |
|---|---|
| Product | [PRD](Docs/PRD.md) · [Business Plan](Docs/Business_Plan.md) · [Project Plan](Docs/Project_Plan_Timeline.md) · [RACI](Docs/RACI_Matrix.md) |
| Design | [Design System](Docs/Design_System.md) · [Screens Repository](Docs/Screens_Repository.md) · [Wireframes](Docs/Wireframe_Specifications.md) |
| Technical | [System Design](Docs/System_Design_Architecture.md) · [ERD](Docs/ERD.md) · [Data Dictionary](Docs/Data_Dictionary.md) · [API Contract](Docs/API_Contract.md) · [Test Plan](Docs/Test_Plan.md) · [DevOps Runbook](Docs/DevOps_Infra_Runbook.md) · [Security](Docs/Security_Compliance.md) · [Integrations](Docs/Third_Party_Integration_Specs.md) · [Analytics](Docs/Event_Tracking_Analytics_Spec.md) |
| Legal/Ops | [ToS & Privacy](Docs/Terms_of_Service_Privacy_Policy.md) · [Supplier Agreement](Docs/Supplier_Wholesaler_Agreement_Template.md) · [Support Playbook](Docs/Customer_Support_Playbook.md) · [Agent Playbook](Docs/Field_Sales_Agent_Playbook.md) |

---

## 🗓️ Roadmap

- **Phase 0 — Foundation (now):** docs ✅ · vendor selection · legal review · hi-fi Figma · hire team
- **Phase 1 — MVP (10-14w):** infra/auth/DB → catalog+browse → cart/checkout+sub-orders → supplier dashboard → delivery → admin → QA/UAT (COD only, single pilot city)
- **Phase 2 — Pilot & Payments (6-8w):** production deploy → card/wallet → agent app → analytics
- **Phase 3 — Scale (8-12w):** BNPL → promotions → multi-city readiness → load testing

See `Project_Plan_Timeline.md` for sprint breakdown.

---

## 🚀 Getting Started

```bash
# clone
 git clone https://github.com/iiYouseFF/Wasla.git
 cd Wasla

# install (once apps scaffolded)
 npm install
 npm run dev  # turborepo
```

Prerequisites: Node 20+, npm/pnpm, (later) React Native env, Docker for local Postgres/Redis.

---

## 🤝 Workflow

- **Issues** are the source of truth — every task is a GitHub Issue linked to a Project board
- **Branching:** `main` is protected · branch from `main` as `feat/<issue>-short-name` · PR required
- **Commits:** conventional commits (`feat:`, `fix:`, `chore:`, `docs:`)
- **Project board:** GitHub Projects (see Projects tab) — Backlog → Todo → In Progress → In Review → Done

See [CONTRIBUTING.md](CONTRIBUTING.md) and issue templates.

---

## 🔗 Key Links

- Backend API repo: [Wasla-backend](https://github.com/iiYouseFF/Wasla-backend)
- Notion HQ: Wasla HQ Dashboard (private)
- Trello: Walsa board (legacy — migrating to GitHub Projects)

---

*Status: Phase 0 — pre-development. Docs complete, awaiting vendor & design finalization before Phase 1 sprints.*
