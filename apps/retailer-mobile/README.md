# Retailer Mobile App

React Native (TypeScript) — iOS + Android. Core loop: browse → cart → checkout (COD) → track.

See `../../Docs/PRD.md` (R1-R12), `Docs/Screens_Repository.md` §1, `Docs/Wireframe_Specifications.md` §1-5.

```
retailer-mobile/
  src/
    screens/   # onboarding, home, search, product, cart, checkout, orders, profile
    components/ # shared UI (from @wasla/ui-components)
    navigation/
    store/     # state (Zustand/Redux)
    api/       # typed client for Wasla-backend /api/v1
    i18n/      # ar / en
```

MVP scope (Phase 1): onboarding (OTP), home/browse/search, cart/checkout (COD), order history/tracking. BNPL/promotions deferred to Phase 2/3.
