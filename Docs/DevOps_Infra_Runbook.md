# DevOps / Infrastructure Runbook & Disaster Recovery Plan
## B2B FMCG Marketplace App

**Version:** 1.0
**Status:** Draft

---

## 1. Environments

| Environment | Purpose | Access |
|---|---|---|
| `local` | Developer machines | Developers only |
| `staging` | Integration testing, QA, UAT | Dev/QA team, pilot testers (limited) |
| `production` | Live system | Restricted, audit-logged access |

**Rule:** Production access requires MFA and is logged. No direct production database writes outside of the application layer except for emergency, approved, logged interventions.

---

## 2. Infrastructure Overview

- **Cloud provider:** AWS or GCP (finalize per System Design open questions)
- **Compute:** Containerized services (Docker), orchestrated via ECS/Fargate or Kubernetes as scale requires — start simple (managed container service) before adopting full Kubernetes complexity
- **Database:** Managed PostgreSQL (e.g., RDS/Cloud SQL) with automated backups and read replica for analytics queries
- **Cache:** Managed Redis (e.g., ElastiCache/Memorystore)
- **Object storage:** S3/Cloud Storage with lifecycle policies and encryption
- **CDN:** CloudFront/Cloud CDN in front of static assets and product images
- **Search:** Managed Elasticsearch/OpenSearch (if adopted) or Postgres full-text at MVP

---

## 3. Deployment Process

1. Developer opens PR → CI runs unit tests, API contract tests, dependency vulnerability scan
2. PR approved and merged to `main`
3. CI/CD pipeline (GitHub Actions) builds container image, tags with commit SHA
4. Auto-deploy to `staging`
5. Manual smoke test / QA sign-off on `staging`
6. Manual promotion to `production` via approved deployment (recommend requiring at least one engineer approval separate from the author)
7. Post-deploy automated health check; rollback triggered automatically if health check fails

### 3.1 Rollback Procedure
- Keep the previous container image tagged and ready for immediate redeploy
- Database migrations must be backward-compatible where possible (avoid destructive migrations in the same release as application code that depends on them — use expand/contract pattern)
- Rollback decision owner: on-call engineer or engineering lead, documented in incident log if triggered

---

## 4. Monitoring & Alerting

| Layer | Tool | Key Metrics/Alerts |
|---|---|---|
| Application errors | Sentry | Error rate spike, new error type introduced |
| Infrastructure metrics | Datadog / Grafana + Prometheus | CPU/memory/disk usage, request latency, error rate (5xx) |
| Uptime | Pingdom/UptimeRobot or cloud-native health checks | API Gateway availability |
| Business-critical alerts | Custom alerts | Payment webhook failures, order creation failure rate spike, stock oversell detection |
| Logs | Centralized logging (CloudWatch/Stackdriver or ELK stack) | Searchable, retained per compliance requirement |

**On-call rotation:** define an on-call schedule (even informal at MVP stage) with a clear escalation path and target response times per severity (align with Test Plan §7 severity definitions).

---

## 5. Backup & Recovery

| Data | Backup Frequency | Retention | Recovery Target (RTO/RPO) |
|---|---|---|---|
| Primary database | Automated daily snapshots + continuous transaction log backup | 30 days minimum (confirm against financial record retention law) | RPO: < 15 min (point-in-time recovery), RTO: < 2 hours |
| Object storage (images, KYC docs) | Versioning enabled, cross-region replication recommended | Per compliance retention requirement | RTO: < 1 hour |
| Configuration/secrets | Version-controlled infra-as-code (Terraform/CDK) + secrets manager backup | N/A (rebuildable from code) | RTO: < 4 hours for full environment rebuild |

**Backup testing:** perform a scheduled quarterly restore drill to confirm backups are actually restorable — an untested backup is not a real backup.

---

## 6. Disaster Recovery Plan

### 6.1 Disaster Scenarios Covered
- Database corruption/loss
- Full region/availability zone outage
- Accidental destructive deployment (bad migration, data deletion)
- Security breach requiring full credential rotation and system isolation

### 6.2 Recovery Steps (General Framework)
1. **Detect & declare**: on-call engineer confirms outage/disaster scope, declares incident, notifies engineering lead/founders
2. **Communicate**: internal status updates on a defined cadence; external status page update if user-facing outage exceeds a defined threshold (e.g., 15 min)
3. **Contain**: isolate affected systems if security-related; halt further writes if data corruption in progress
4. **Recover**: restore from most recent clean backup per §5 targets; validate data integrity before resuming traffic
5. **Resume**: gradually restore traffic (feature flag or canary rollout if possible) rather than full immediate cutover
6. **Post-mortem**: blameless post-mortem within 5 business days, documented root cause and action items

### 6.3 Multi-Region Consideration
At MVP/single-city scale, a single-region deployment with strong backup/restore discipline is sufficient. Revisit multi-region active-active or active-passive architecture once the platform expands to multiple countries or uptime SLAs tighten (e.g., enterprise supplier contracts requiring 99.9%+ uptime commitments).

---

## 7. Infrastructure-as-Code

- All infrastructure defined in Terraform (or AWS CDK/Pulumi) — no manual console-created resources in production
- Environment parity: `staging` and `production` should use the same IaC modules with different parameter values, to catch environment-specific bugs before production deploy

---

## 8. Secrets & Configuration Management

- All API keys, database credentials, and third-party integration secrets stored in a secrets manager, injected at runtime — never committed to source control
- Rotate secrets on a defined schedule and immediately upon suspected compromise or team member offboarding

---

## 9. Scaling Playbook (Reference for Future Growth)

| Trigger | Action |
|---|---|
| API latency degrading under load | Horizontal scale of API containers; review slow query logs |
| Database CPU/connection saturation | Add read replicas for analytics/reporting queries; review indexing |
| Search latency increasing | Move from Postgres full-text to dedicated Elasticsearch/OpenSearch cluster |
| Growing multi-city complexity | Consider splitting the modular monolith into the microservices boundaries already defined in System Design doc (Order, Inventory, Payment, etc. as independent services) |

---

## 10. Open Items
- Finalize cloud provider (AWS vs GCP) — blocks finalizing this doc's provider-specific tooling names
- Define formal on-call rotation and response SLAs once team is staffed
- Confirm data retention periods per local financial/tax law to finalize backup retention policy
- Schedule first backup restore drill before production launch, not after
