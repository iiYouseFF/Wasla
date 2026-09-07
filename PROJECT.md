# Wasla — GitHub Project Workflow

**Project board:** [Wasla — Roadmap](https://github.com/users/iiYouseFF/projects/1) · `PVT_kwHOCcbIYc4Bit1U`

This project spans both repos: [`Wasla`](https://github.com/iiYouseFF/Wasla) (apps + docs + design) and [`Wasla-backend`](https://github.com/iiYouseFF/Wasla-backend) (API). Issues from both repos appear in the same board.

---

## Board Structure

### Status (columns — `Status` field)

| Column | Meaning | When to move |
|---|---|---|
| **Backlog** (`Backlog`) | Not yet planned for current sprint | New issues default here if Phase 1+ |
| **Todo** (`Todo`) | Ready, prioritized for this sprint | Phase 0 + Sprint 1-2 kickoff |
| **In Progress** (`In Progress`) | Active work, branch exists | Assign → branch `feat/<id>-name` |
| **In Review** (`In Review`) | PR open, awaiting review | Move when PR created |
| **Done** (`Done`) | Merged to `main`, deployed to staging | Auto when PR merges (`Closes #`) |

**WIP rule:** keep `In Progress` + `In Review` ≤ team size; finish before starting.

### Sprint (`Sprint` field)

`Phase 0` → `Sprint 1-2` (infra/auth/DB) → `3-4` (catalog/browse) → `5-6` (cart/orders) → `7` (supplier) → `8` (delivery) → `9` (admin) → `10` (QA/UAT). Phase 2/3 issues have no Sprint yet (use `phase:` labels for filtering).

### Views

- **Table — All Issues** (`PVTV_lAHOCcbIYc4Bit1UzgLkjNA`) — flat list, sort by Sprint then Priority
- **Board — by Status** (`PVTV_lAHOCcbIYc4Bit1UzgLkjTU`) — Kanban grouped by `Status` (Backlog→Done). *Configure in UI: Board → Group by → Status if not auto-grouped.*
- **Roadmap — by Sprint** (`PVTV_lAHOCcbIYc4Bit1UzgLkjTY`) — table grouped/sorted by `Sprint` field; use for sprint planning

---

## Labels

- `type: epic|feature|task|bug|chore|docs` — *what* it is
- `priority: P0|P1|P2|P3` — P0 critical (Phase 0), P1 high (MVP), P2 medium, P3 low
- `app: retailer|supplier|agent|admin|backend` — *where* it ships
- `area: infra|design|product|api` — workstream
- `phase: 0|1-mvp|2|3` + `sprint: 1-2 ... 10` — roadmap position
- Use both `status` field *and* labels — field drives board columns, labels drive filters.

---

## Milestones (repo-level, mirrored)

| # | Title | Due | Scope |
|---|---|---|---|
| 4 | Phase 0 - Foundation | 2026-09-30 | Vendor + legal + Figma + team (Wasla#1-4) |
| 1 | Phase 1 — MVP Build (Sprints 1-10) | 2026-12-31 | Core loop COD, single city (Wasla#5-13 + backend#1-7) |
| 2 | Phase 2 — Pilot & Payments | 2027-03-15 | Card/wallet, agent app, analytics |
| 3 | Phase 3 — Scale & BNPL | 2027-06-30 | BNPL, promotions, multi-city |

Close milestone only when **all** its issues are `Done` + exit criteria verified (see `Project_Plan_Timeline.md §8`).

---

## Issue → Branch → PR → Done

1. **Pick** top `Todo` (or `Backlog` → `Todo` in planning). Assign yourself; move to `In Progress`.
2. **Branch** from `main`: `feat/7-retailer-onboarding` or `fix/12-cart-moq` (use issue number + kebab).
3. **Commit** conventional: `feat: add OTP verify screen (#7)` + small, reviewable commits.
4. **PR**: template, `Closes #7`, link Figma/docs, add screenshots. Move project item to `In Review`.
5. **CI** must pass (lint, typecheck, unit, contract tests, audit). Request 1 review.
6. **Merge** via *Squash & merge*; issue auto-closes; project auto → `Done` (verify).

**Docs as code:** schema/contract changes (`Data_Dictionary.md`, `API_Contract.md`, `ERD.md`) must be updated in the *same PR* — never drift.

---

## Automation (recommended next)

- **Auto-add to project:** already linked repos (`linkProjectV2ToRepository`) — new issues auto-appear in `Backlog`/`Todo` (check Project Settings → Workflows → Auto-add).
- **Branch protection** (`main`): require PR, require status checks (CI), dismiss stale approvals, no force push. Set via `Settings → Branches → Add rule`.
- **Convert `Todo → In Progress` on branch creation:** Project workflow `When: Item added → Set Status = Backlog` already; add workflow `When: Issue assigned → Set Status = In Progress`.

---

## Quick Links

- Wasla issues: https://github.com/iiYouseFF/Wasla/issues
- Wasla-backend issues: https://github.com/iiYouseFF/Wasla-backend/issues
- Docs: [`Docs/`](Docs/) + Notion HQ
- Project board: https://github.com/users/iiYouseFF/projects/1
