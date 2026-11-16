# Initial Technical Architecture

## System shape

A modular monolith backend serves three clients:

- Web: Next.js PWA with IndexedDB.
- Desktop: Electron with a local SQLite database.
- Mobile: React Native with a local SQLite database.
- API: Node.js and TypeScript.
- Central database: PostgreSQL.
- Background jobs: Redis and BullMQ when asynchronous workloads are introduced.

The three clients share API contracts, domain types, validation rules, permissions semantics and sync protocol. Platform-specific storage and presentation are implemented behind adapters.

## Multi-tenancy

The initial ownership hierarchy is:

Platform → Tenant → Company → Branch → Warehouse / POS device.

The server derives tenant scope from the authenticated membership. It must never trust a client-supplied tenant ID as proof of access. PostgreSQL row-level security and tenant-aware foreign keys are part of the security design.

## Device-first offline model

Each device has:
- A stable registered device ID.
- A local database.
- A durable outbox of commands created offline.
- An inbox for server changes.
- A per-stream or per-tenant sync cursor.
- Idempotency keys for every mutation.
- A visible state for pending, accepted, rejected and conflict records.

The server validates and applies commands transactionally. Replayed commands return the original result rather than applying twice. Stock is derived from an immutable movement ledger, not reconciled by overwriting a quantity field.

## Conflict policy

- Financial postings: never last-write-wins; reject or require an explicit reversal/correction workflow.
- Stock: preserve every movement and detect overselling or invalid negative stock according to tenant policy.
- Product descriptions and low-risk settings: may use version checks and explicit conflict resolution.
- User permissions and device revocation: server-authoritative.
- Online payment settlement: only mark settled after trusted provider confirmation.

## Initial backend modules

1. Core: tenants, companies, branches and settings.
2. Identity: users, memberships, roles and permissions.
3. Devices: registration, revocation and device sessions.
4. Subscriptions: plans, modules and entitlements.
5. Sync: command intake, event delivery, cursors and conflicts.
6. Audit: security and business activity records.

Retail modules follow after the foundation: catalog, inventory, sales, procurement and CRM.

## Quality gates

Before pilot release, test tenant isolation, device revocation, idempotent replay, network interruption, restart recovery, simultaneous stock sales, migration rollback/forward strategy, backups and restore, and audit traceability.
