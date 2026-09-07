# @wasla/shared-types

TypeScript types shared across apps and Wasla-backend (ERD/Data Dictionary as source). Published as workspace package.

```ts
// example
export type UserRole = 'retailer' | 'supplier' | 'wholesaler' | 'sales_agent' | 'admin';
export type OrderStatus = 'placed' | 'confirmed' | 'partially_fulfilled' | 'completed' | 'cancelled';
```

Keep in sync with `Docs/Data_Dictionary.md` and `Docs/API_Contract.md`.
