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

---

## 2. Authentication

### POST /auth/otp/request
Request an OTP for phone-based login/registration.

**Request**
```json
{
  "phone_number": "+201001234567"
}
```
**Response `200`**
```json
{
  "data": {
    "otp_sent": true,
    "expires_in": 300
  }
}
```

### POST /auth/otp/verify
**Request**
```json
{
  "phone_number": "+201001234567",
  "otp_code": "482913"
}
```
**Response `200`**
```json
{
  "data": {
    "access_token": "eyJhbGciOi...",
    "refresh_token": "eyJhbGciOi...",
    "user": {
      "user_id": "uuid",
      "role": "retailer",
      "is_profile_complete": false
    }
  }
}
```

### POST /auth/refresh
**Request**
```json
{ "refresh_token": "eyJhbGciOi..." }
```
**Response `200`**
```json
{ "data": { "access_token": "eyJhbGciOi..." } }
```

### POST /auth/logout
**Headers:** `Authorization: Bearer <token>`
**Response `204`** No content

---

## 3. Error Handling

**Standard error envelope**
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "phone_number is required",
    "details": [
      { "field": "phone_number", "issue": "required" }
    ]
  }
}
```

**Common error codes**

| HTTP Status | Code | Meaning |
|---|---|---|
| 400 | `VALIDATION_ERROR` | Request body failed validation |
| 401 | `UNAUTHORIZED` | Missing/invalid/expired token |
| 403 | `FORBIDDEN` | Authenticated but not authorized for this action |
| 404 | `NOT_FOUND` | Resource doesn't exist |
| 409 | `CONFLICT` | e.g. stock insufficient, duplicate resource |
| 422 | `UNPROCESSABLE` | Business rule violation (e.g. below MOQ) |
| 429 | `RATE_LIMITED` | Too many requests (e.g. OTP spam) |
| 500 | `INTERNAL_ERROR` | Unexpected server error |

---

## 4. User & Profile

### GET /users/me
Returns current authenticated user + role-specific profile.

**Response `200`**
```json
{
  "data": {
    "user_id": "uuid",
    "phone_number": "+201001234567",
    "role": "retailer",
    "retailer": {
      "retailer_id": "uuid",
      "business_name": "Al Amal Mini Market",
      "business_type": "grocery",
      "city_id": "uuid",
      "credit_tier": "tier_2"
    }
  }
}
```

### PATCH /users/me
**Request**
```json
{
  "business_name": "Al Amal Mini Market",
  "business_type": "grocery",
  "address": "12 Tahrir St, Dokki",
  "location_lat": 30.0401,
  "location_lng": 31.2077
}
```
**Response `200`** → updated profile object

---

## 5. Catalog

### GET /categories
**Response `200`**
```json
{
  "data": [
    { "category_id": "uuid", "name": "Beverages", "parent_category_id": null },
    { "category_id": "uuid", "name": "Carbonated Drinks", "parent_category_id": "uuid" }
  ]
}
```

### GET /products
**Query params:** `category_id`, `search`, `city_id`, `page`, `limit`

**Response `200`**
```json
{
  "data": [
    {
      "product_id": "uuid",
      "name": "Cola 330ml Can (Pack of 24)",
      "brand": "BrandCo",
      "category_id": "uuid",
      "unit_of_measure": "carton",
      "image_url": "https://cdn.platform.com/products/xyz.jpg",
      "lowest_price": 240.00,
      "listing_count": 3
    }
  ],
  "meta": { "page": 1, "limit": 20, "total": 87 }
}
```

### GET /products/:product_id/listings
Returns all seller listings for a given product (price comparison view).

**Response `200`**
```json
{
  "data": [
    {
      "listing_id": "uuid",
      "seller_id": "uuid",
      "seller_type": "wholesaler",
      "seller_name": "Cairo Wholesale Co.",
      "price": 240.00,
      "min_order_qty": 2,
      "stock_available": 150,
      "rating": 4.6
    },
    {
      "listing_id": "uuid",
      "seller_id": "uuid",
      "seller_type": "supplier",
      "seller_name": "BrandCo Direct",
      "price": 235.50,
      "min_order_qty": 5,
      "stock_available": 500,
      "rating": 4.8
    }
  ]
}
```

### GET /listings/:listing_id
Full listing detail (product info + seller info + stock + reviews summary).

---

## 6. Cart

### GET /cart
Returns current retailer's active cart, grouped by seller.

**Response `200`**
```json
{
  "data": {
    "cart_id": "uuid",
    "groups": [
      {
        "seller_id": "uuid",
        "seller_name": "Cairo Wholesale Co.",
        "items": [
          {
            "cart_item_id": "uuid",
            "listing_id": "uuid",
            "product_name": "Cola 330ml Can (Pack of 24)",
            "quantity": 4,
            "unit_price": 240.00,
            "line_total": 960.00
          }
        ],
        "subtotal": 960.00,
        "meets_moq": true
      }
    ],
    "grand_total": 960.00
  }
}
```

### POST /cart/items
**Request**
```json
{ "listing_id": "uuid", "quantity": 4 }
```
**Response `200`** → updated cart object

### PATCH /cart/items/:cart_item_id
**Request**
```json
{ "quantity": 6 }
```

### DELETE /cart/items/:cart_item_id
**Response `204`**

---

## 7. Orders

### POST /orders
Creates an order from the current cart. Splits into sub-orders per seller automatically.

**Headers:** `Idempotency-Key: <client-generated-uuid>`

**Request**
```json
{
  "payment_method": "cod",
  "delivery_address_id": "uuid",
  "bnpl": false
}
```
**Response `201`**
```json
{
  "data": {
    "order_id": "uuid",
    "order_status": "placed",
    "total_amount": 1450.00,
    "sub_orders": [
      {
        "sub_order_id": "uuid",
        "seller_id": "uuid",
        "seller_name": "Cairo Wholesale Co.",
        "status": "pending",
        "subtotal_amount": 960.00,
        "items": [
          { "listing_id": "uuid", "product_name": "Cola 330ml Can", "quantity": 4, "unit_price": 240.00 }
        ]
      }
    ],
    "created_at": "2026-09-06T10:00:00Z"
  }
}
```

**Error `409`** — insufficient stock
```json
{
  "error": {
    "code": "CONFLICT",
    "message": "Insufficient stock for listing uuid",
    "details": [{ "listing_id": "uuid", "requested": 10, "available": 4 }]
  }
}
```

### GET /orders
**Query params:** `status`, `page`, `limit`
Returns retailer's order list (or seller's incoming sub-orders, scoped by role).

### GET /orders/:order_id
Full order detail including all sub-orders, payment, and delivery status.

### PATCH /suborders/:sub_order_id/status
**Role:** supplier/wholesaler only

**Request**
```json
{ "status": "confirmed" }
```
Valid transitions enforced server-side: `pending → confirmed → packed → out_for_delivery → delivered → completed`, or `→ cancelled` from early states.

**Response `200`** → updated sub-order object

### POST /orders/:order_id/cancel
**Request**
```json
{ "reason": "retailer_requested" }
```

---

## 8. Payments

### POST /payments
Initiates payment for an order (called internally by order creation, or separately for retry).

**Request**
```json
{
  "order_id": "uuid",
  "method": "card"
}
```
**Response `200`**
```json
{
  "data": {
    "payment_id": "uuid",
    "status": "pending",
    "redirect_url": "https://checkout.psp.com/session/xyz"
  }
}
```

### POST /payments/webhook (PSP → Platform)
Payment gateway callback. Verifies signature, updates payment + order status.

### GET /payments/:payment_id

---

## 9. BNPL

### GET /bnpl/account
**Response `200`**
```json
{
  "data": {
    "bnpl_account_id": "uuid",
    "credit_limit": 5000.00,
    "available_credit": 3200.00,
    "status": "active",
    "next_repayment": {
      "amount": 800.00,
      "due_date": "2026-09-20"
    }
  }
}
```

### POST /bnpl/repayments
**Request**
```json
{ "bnpl_transaction_id": "uuid", "amount": 800.00, "method": "wallet" }
```

### GET /bnpl/transactions
Paginated list of draws/repayments.

---

## 10. Delivery

### GET /deliveries/:sub_order_id/track
**Response `200`**
```json
{
  "data": {
    "delivery_id": "uuid",
    "status": "out_for_delivery",
    "driver_name": "Ahmed S.",
    "estimated_delivery_time": "2026-09-06T14:00:00Z",
    "current_location": { "lat": 30.05, "lng": 31.23 }
  }
}
```

---

## 11. Reviews

### POST /reviews
**Request**
```json
{
  "sub_order_id": "uuid",
  "rating": 5,
  "comment": "Fast delivery, good packaging."
}
```

### GET /listings/:listing_id/reviews

---

## 12. Promotions

### GET /promotions
Public/retailer-facing active promotions (query by `city_id`, `category_id`)

### POST /promotions
**Role:** supplier/wholesaler

**Request**
```json
{
  "listing_id": "uuid",
  "discount_type": "percentage",
  "discount_value": 10,
  "start_date": "2026-09-10",
  "end_date": "2026-09-20"
}
```

---

## 13. Analytics (Supplier/Admin)

### GET /analytics/supplier/:seller_id/summary
**Query params:** `start_date`, `end_date`

**Response `200`**
```json
{
  "data": {
    "gmv": 125000.00,
    "order_count": 340,
    "top_products": [
      { "product_id": "uuid", "name": "Cola 330ml Can", "units_sold": 1200 }
    ],
    "demand_by_city": [
      { "city_id": "uuid", "city_name": "Giza", "order_count": 210 }
    ]
  }
}
```

### GET /analytics/admin/platform-summary
**Role:** admin only — platform-wide KPIs (GMV, active users, fulfillment rate)

---

## 14. Admin — User Management

### GET /admin/accounts?role=supplier&status=pending
### PATCH /admin/accounts/:user_id/approve
### PATCH /admin/accounts/:user_id/suspend

---

## 15. Notifications

### GET /notifications
Paginated notification inbox for current user.

### PATCH /notifications/:notification_id/read

---

## 16. Webhooks (Outbound, Platform → Partners)

| Event | Payload trigger |
|---|---|
| `order.created` | New order placed |
| `suborder.status_changed` | Sub-order status transition |
| `payment.completed` | Payment confirmed |
| `bnpl.repayment_due` | Repayment reminder trigger for external fintech sync |

---

## 17. Rate Limits (Recommended Defaults)

| Endpoint group | Limit |
|---|---|
| OTP request | 5 requests / phone / hour |
| General authenticated API | 100 requests / minute / user |
| Search endpoints | 30 requests / minute / user |

---

## 18. Open Items for Backend Team
- Finalize exact enum values for `order_status` / `sub_order_status` with the ops team
- Confirm whether listing price includes tax or tax is calculated separately at checkout
- Define exact BNPL partner API contract once provider is selected (this spec assumes an abstracted internal interface)
- Decide on GraphQL for admin/analytics if REST pagination/filtering proves insufficient
