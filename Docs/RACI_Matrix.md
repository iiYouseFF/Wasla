# RACI Matrix
## B2B FMCG Marketplace App

**Version:** 1.0
**Status:** Draft — role names are functional placeholders; map to actual team members once hired

**Legend:** R = Responsible (does the work) · A = Accountable (owns the outcome, final approver) · C = Consulted (input sought) · I = Informed (kept updated)

---

## 1. Roles Referenced

| Role | Description |
|---|---|
| Founder/CEO | Overall business accountability |
| Product Manager | Product scope, prioritization |
| Tech Lead / CTO | Technical architecture and delivery |
| Backend Engineers | API/backend implementation |
| Mobile Engineers | Retailer/Agent app implementation |
| Web Engineers | Supplier dashboard/Admin panel implementation |
| Designer | UI/UX design |
| QA | Testing and quality assurance |
| DevOps | Infrastructure, deployment, monitoring |
| Ops/Support Lead | Customer support, field agent management |
| Finance Lead | Payments, payouts, BNPL financial terms |
| Legal Counsel | Contracts, compliance, regulatory review |
| Marketing Lead | Promotions, retailer/supplier acquisition campaigns |

---

## 2. Product & Planning

| Activity | Founder | PM | Tech Lead | Designer | Legal | Finance |
|---|---|---|---|---|---|---|
| Define product requirements (PRD) | C | A/R | C | C | I | I |
| Prioritize feature roadmap | A | R | C | C | I | I |
| Approve business model changes | A | R | I | I | C | C |

---

## 3. Design

| Activity | PM | Designer | Tech Lead | Mobile Eng | Web Eng |
|---|---|---|---|---|---|
| Wireframes/screens repository | C | A/R | I | C | C |
| High-fidelity UI design (Figma) | C | A/R | I | C | C |
| Design system/style guide | C | A/R | C | C | C |

---

## 4. Engineering & Technical

| Activity | Tech Lead | Backend Eng | Mobile Eng | Web Eng | DevOps | QA |
|---|---|---|---|---|---|---|
| System architecture design | A/R | C | C | C | C | I |
| API contract definition | A | R | C | C | I | C |
| Database schema implementation | A | R | I | I | C | I |
| Mobile app development | A | I | R | I | I | C |
| Web dashboard development | A | I | I | R | I | C |
| Infra setup & CI/CD pipeline | A | C | I | I | R | I |
| Security review | A | R | R | R | C | C |
| Test plan execution | A | C | C | C | I | R |
| Production deployment | A | R | I | I | R | C |
| Incident response (technical) | A/R | R | C | C | R | I |

---

## 5. Third-Party & Vendor Management

| Activity | Founder | Tech Lead | Finance | Legal |
|---|---|---|---|---|
| Payment gateway (PSP) selection | A | R | C | C |
| BNPL/fintech partner selection | A | C | R | C |
| Logistics partner selection (per city) | C | C | R | I |
| Cloud provider selection | I | A/R | C | I |

---

## 6. Legal & Compliance

| Activity | Founder | Legal | PM | Finance |
|---|---|---|---|---|
| Terms of Service finalization | A | R | C | I |
| Privacy Policy finalization | A | R | C | I |
| Supplier/Wholesaler agreement finalization | A | R | C | C |
| BNPL regulatory/licensing review | A | R | I | C |
| Data protection compliance review | A | R | I | I |

---

## 7. Operations

| Activity | Ops Lead | Founder | Support Team | Field Agents | Marketing |
|---|---|---|---|---|---|
| Customer support operations | A/R | I | R | I | I |
| Field agent hiring & training | A/R | C | I | R | C |
| Retailer onboarding execution | A | I | I | R | C |
| Supplier/wholesaler account approval | A | I | C | I | I |
| Dispute resolution | A/R | I | R | I | I |

---

## 8. Marketing & Growth

| Activity | Marketing Lead | Founder | PM | Field Agents |
|---|---|---|---|---|
| Promotions/campaign design | A/R | C | C | I |
| Retailer acquisition campaigns | A/R | C | I | C |
| Supplier partnership outreach | A/R | A | C | I |

---

## 9. Finance

| Activity | Finance Lead | Founder | Ops Lead | Legal |
|---|---|---|---|---|
| Commission/fee structure setting | A/R | A | C | C |
| Payout processing | A/R | I | C | I |
| BNPL credit risk policy | A/R | C | I | C |
| Financial reporting/investor updates | A/R | A | I | I |

---

## 10. Notes on Using This Matrix

- Every activity should have exactly **one** "A" (Accountable) — if two people are marked "A" for the same row, resolve that ambiguity before work starts, since dual accountability tends to produce dropped ownership in practice
- Update this matrix once actual team members are hired — replace functional role names with real names/roles
- Revisit quarterly as the org grows past founder-led operations into dedicated functional teams
