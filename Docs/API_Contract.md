# API Contract
## B2B FMCG Marketplace App

**Version:** 1.0
**Status:** Draft
**Base URL:** `https://api.platform.com/api/v1`

---

## 1. Conventions

- **Format:** JSON request/response bodies
- **Auth:** Bearer token (JWT) in `Authorization: Bearer <token>` header, except for OTP request/verify endpoints
- **Versioning:** URI-based (`/api/v1/...`)
- **Pagination:** cursor or offset-based via query params `?page=1&limit=20`, responses include a `meta` block
- **Dates:** ISO 8601 (`2026-09-06T10:00:00Z`)
- **IDs:** UUID strings
- **Errors:** standard error envelope (see §3)
- **Idempotency:** mutating endpoints on order/payment accept an optional `Idempotency-Key` header to make retries safe

### Standard success envelope
```json
{
  "data": { },
  "meta": {
    "page": 1,
    "limit": 20,
    "total": 143
  }
}
```
