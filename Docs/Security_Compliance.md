# Security & Compliance
## B2B FMCG Marketplace App

**Version:** 1.0
**Status:** Draft — treat regulatory items as flags for legal/compliance counsel, not final legal determinations

---

## 1. Purpose

Defines the security controls and compliance obligations the platform must meet, given it handles PII, business KYC data, and payment/credit (BNPL) information. This complements the Privacy Policy (legal-facing) with engineering-facing controls.

---

## 2. Data Classification

| Class | Examples | Handling Requirement |
|---|---|---|
| **Restricted** | Payment card data, BNPL/credit data, KYC documents, bank account details | Encrypted at rest and in transit, access logged, least-privilege access only |
| **Confidential** | Phone numbers, business registration numbers, addresses, location data | Encrypted at rest, access restricted to relevant roles/services |
| **Internal** | Order data, catalog/pricing, analytics events | Standard access controls, not publicly exposed |
| **Public** | Product images/descriptions, promotional content | No special handling |

---

## 3. Authentication & Access Control

- OTP-based authentication with rate limiting (see API Contract §17) to prevent brute-force/abuse
- JWT access tokens short-lived (recommend 15-60 min), refresh tokens longer-lived and revocable
- Role-based access control (RBAC) enforced server-side on every endpoint — never rely on client-side role checks alone
- Admin panel requires 2FA in addition to password/OTP given elevated privileges (account approval, payouts, dispute resolution)
- Principle of least privilege for internal staff/admin roles — segment permissions (e.g., support staff can view but not approve payouts)

---

## 4. Payment Security (PCI-DSS Scope Minimization)

- **Never store raw card numbers, CVV, or full magnetic stripe/chip data** on platform servers
- Use PSP-hosted checkout pages or client-side tokenization (e.g., Stripe Elements) so card data never touches the platform backend — this keeps the platform in the lowest PCI-DSS SAQ scope (SAQ-A or similar) rather than full PCI compliance
- Store only tokenized payment references and last-4-digits for display purposes
- All payment-related API traffic over TLS 1.2+

---

## 5. Encryption Standards

| Data State | Standard |
|---|---|
| In transit | TLS 1.2+ for all client-server and server-server communication |
| At rest (database) | AES-256 encryption for `Restricted` and `Confidential` data classes (application-level or database-level encryption for sensitive columns: bank details, KYC doc references) |
| At rest (object storage) | Server-side encryption enabled on all buckets; KYC document bucket private with signed-URL access only |
| Secrets management | API keys/credentials in a secrets manager (e.g., AWS Secrets Manager/HashiCorp Vault), never hardcoded or committed to source control |

---

## 6. KYC / AML Considerations

- Supplier/Wholesaler onboarding requires business registration verification before `kyc_status = verified`
- If BNPL involves underwriting individual retailers, confirm with legal counsel whether AML (Anti-Money Laundering) obligations apply given the embedded finance component
- Maintain audit trail of who approved/rejected each KYC submission and when (see Data Dictionary — extend `Payout`/account approval events with an audit log table if not already covered)

---

## 7. Audit Logging

Maintain immutable audit logs for:
- Admin account approvals/suspensions
- Payout processing actions
- Dispute resolution decisions
- BNPL credit limit changes
- Any direct database modification by internal staff (should be rare and always logged)

Audit logs should capture: actor (`user_id`), action, target entity, before/after values where applicable, timestamp, and IP address.

---

## 8. Vulnerability Management

- Dependency scanning on every build (e.g., `npm audit`, Snyk, or GitHub Dependabot)
- OWASP Top 10 review as part of code review checklist for any new endpoint (injection, broken auth, XSS, insecure deserialization, etc.)
- Scheduled penetration test before public launch and at least annually thereafter, with particular focus on the payment and BNPL flows
- Bug bounty or responsible disclosure channel (even a simple `security@platform.com` inbox) before public launch

---

## 9. Infrastructure Security

- Network segmentation: database and internal services not directly internet-exposed; only API Gateway public-facing
- Web Application Firewall (WAF) in front of public endpoints
- Rate limiting and DDoS protection at the edge/CDN layer
- Regular automated backups with tested restore procedure (see DevOps/Infra Runbook)
- Least-privilege IAM roles for all cloud service accounts; no shared/root credentials for day-to-day operations

---

## 10. Regulatory Compliance Checklist (Flag for Legal/Compliance Counsel)

- [ ] Data protection law applicability (local equivalent of GDPR — confirm which law applies to target market and what it requires: consent mechanisms, right to erasure, data breach notification timelines)
- [ ] Data residency requirements (some jurisdictions require certain data, especially financial, to remain on local servers)
- [ ] Financial services / lending license requirement for BNPL feature (highest-priority legal item — do not launch BNPL without confirming licensing structure)
- [ ] AML/KYC obligations tied to the BNPL/financial component
- [ ] E-commerce/electronic contract validity law (does the jurisdiction require specific disclosures or digital signature standards for order confirmations?)
- [ ] Consumer protection law scope (does it apply to B2B transactions on this platform, or only the retailer-facing BNPL/consumer-credit angle?)
- [ ] Data breach notification obligations and timelines

---

## 11. Incident Response (High-Level)

1. **Detect**: monitoring/alerting flags anomaly (see DevOps/Infra Runbook for tooling)
2. **Contain**: isolate affected systems, revoke compromised credentials
3. **Assess**: determine scope of data/systems affected
4. **Notify**: legal/compliance determines notification obligations (users, regulators) per applicable law and timelines
5. **Remediate**: patch root cause, rotate all potentially exposed secrets
6. **Post-mortem**: document root cause and preventive measures, shared internally

A detailed incident response runbook with named roles/escalation paths should be built out before launch — this section is a placeholder framework.

---

## 12. Open Items
- Confirm applicable data protection law and appoint a Data Protection Officer or equivalent responsible person if legally required
- Confirm BNPL licensing structure with legal counsel before that feature goes live — this is the single highest compliance risk item in this document
- Schedule a security architecture review with a specialist before handling live payment data at scale
