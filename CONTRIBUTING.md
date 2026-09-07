# Contributing to Wasla

## Workflow (GitHub Issues + Projects)

1. **Pick an issue** from the Project board (Backlog → Todo → In Progress → In Review → Done). Never work without an issue.
2. **Create a branch** from `main`: `feat/12-retailer-onboarding` or `fix/34-cart-moq-check`
3. **Commit** with conventional commits: `feat: add OTP verify screen (#12)`
4. **Open a PR** — link the issue (`Closes #12`), fill the PR template, request review
5. **CI must pass** (lint, typecheck, tests) before merge
6. **Squash & merge** to `main`; issue auto-closes, Project status → Done

## Branch Naming

- `feat/<issue>-kebab-case`
- `fix/<issue>-kebab-case`
- `chore/<issue>-kebab-case`
- `docs/<issue>-kebab-case`

## Code Style

- TypeScript strict, ESLint + Prettier
- No hardcoded UI strings (i18n from day one — Arabic/English + RTL)
- Currency/date formatting locale-aware
- API contracts follow `Docs/API_Contract.md` — versioned `/api/v1`

## Definition of Done

- [ ] Linked issue + Project status updated
- [ ] Tests (unit/integration/API) added or updated
- [ ] Docs updated if contract/schema changed
- [ ] i18n keys added, RTL checked
- [ ] PR approved by 1 reviewer
