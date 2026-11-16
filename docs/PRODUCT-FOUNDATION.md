# Global Business Platform — Product Foundation

## Product direction

Build one modular, multi-tenant business platform. The first commercial release targets Nigerian retail businesses and focuses on commerce, POS, inventory, purchasing and basic delivery operations. Additional modules are activated by subscription and permission.

## Confirmed product decisions

- One unified platform with modules businesses can activate.
- Initial market: Nigeria; initial industry: retail.
- Customer scope: SMEs and larger enterprises.
- Offline synchronisation: each registered device works independently and synchronises directly with the cloud.
- Initial clients: web/PWA, Windows desktop and mobile are all in scope.
- Marketplace direction: vendor advertising and transactional marketplace, introduced after the retail foundation.
- Logistics: own riders and third-party local/international carriers.
- Existing projects must be audited before code is reused.

## Architecture principles

1. One canonical backend and shared domain rules.
2. Three clients; no separate business logic implementations.
3. PostgreSQL is the central system of record.
4. Each device has its own local database and durable outbox.
5. Financial and inventory records are append-only movements or events; do not resolve them with last-write-wins.
6. Every write is authenticated, tenant-scoped, authorised, validated and auditable.
7. Offline writes are provisional until accepted by the server.
8. Third-party services (payment confirmation, carrier quotes, cloud AI) require connectivity unless a provider explicitly supports an offline workflow.

## Initial release boundary

The first retail release should cover tenant/company/branch setup, users and permissions, device registration, products and barcodes, inventory movements, POS sales, receipts, returns, suppliers, purchase orders, goods received, branch transfers, customer records, offline synchronisation and basic reports.

Do not implement every industry, full marketplace settlement, international carrier integrations or autonomous financial AI in the first release.

## Repository policy

- Keep the existing `main` branch untouched while the platform foundation is developed.
- Work on `global-platform-foundation` until the audit establishes what can safely be reused.
- Never commit credentials, production data, private keys or real customer information.
- Use pull requests and automated checks before merging to `main`.
