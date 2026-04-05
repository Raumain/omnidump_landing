# OmniDump — Landing Page Context (LLM Handoff)

## 1. Purpose of this document

This file is a **source-grounded context pack** for generating a public landing page for OmniDump.  
It translates the current codebase into product messaging inputs and includes constraints to avoid inaccurate claims.

## 2. Project snapshot

- **Product:** OmniDump
- **Type:** Self-hosted database operations + ETL developer tool
- **Current app surface:** Connections, Schema, Import
- **Primary stack:** Bun + TanStack Start + React + TanStack Query/Router + Kysely + native DB CLIs
- **Primary deployment mode:** Docker / Docker Compose

## 3. Positioning summary (what OmniDump is)

OmniDump is a self-hosted “database operations cockpit” for developers. It centralizes:
1. Multi-database connection management (Postgres, MySQL, SQLite)
2. Schema exploration/export
3. SQL dump/restore workflows
4. CSV import with mapping and reject reporting
5. Optional SSH tunneling for protected databases

## 4. Likely target audience (inferred)

- Full-stack/backend engineers managing dev/staging/prod-like data
- DevOps/platform engineers handling secure DB access and backups
- Teams needing local/self-hosted tooling (privacy-sensitive or internal infra)
- Developers moving data between environments for debugging/testing

## 5. Core differentiators (source-backed)

1. **Native DB tooling in container runtime**  
   Runtime image installs `pg_dump`, `mysqldump`, `sqlite3`, `openssh-client`.
2. **Self-hosted by default**  
   Connections stored locally in app SQLite; dumps/reject files written to local app storage.
3. **Integrated SSH tunnel flow**  
   Built-in tunnel orchestration with pooling + idle timeout.
4. **End-to-end in one UI**  
   Connection manager + schema explorer + dump/restore + CSV mapping/import.
5. **Streaming import feedback**  
   CSV import streams progress events and produces reject files for failed rows.

## 6. Shipped capabilities inventory

| Capability | Status | Notes for marketing |
|---|---|---|
| Save/list/update/delete DB connections | Shipped | Supports postgres/mysql/sqlite credentials |
| Active connection persistence | Shipped | Stored in `localStorage` |
| Connection test API | Shipped | Includes tunnel-aware test path |
| Schema introspection | Shipped | Table + column metadata displayed |
| Schema export (JSON/DBML/SQL) | Shipped | SQL export uses native dump tools |
| SQL dump generation | Shipped | Saved under `exports/dumps/...` |
| Selective table dump | Partially shipped | Implemented for Postgres/MySQL; SQLite falls back to full dump |
| Immediate dump download | Shipped | Optional during dump creation |
| Dump management (list/download/delete/restore) | Shipped | Drawer-based UX in schema page |
| CSV import with mapping UI | Shipped | Includes auto-mapping heuristics |
| Import progress + reject CSV download | Shipped | SSE stream + reject file endpoint |
| Data wipe/clear/drop actions | Shipped | Includes destructive confirmations |
| Table seeding via Faker | Likely shipped, verify before headline use | Endpoint contract appears inconsistent (see risks) |

## 7. Core user flows (for landing-page storytelling)

### Flow A: Connect and inspect
1. Add DB credentials (optionally SSH).
2. Test and save connection.
3. Select active DB globally.
4. Inspect schema and table details.

### Flow B: Backup and restore
1. Open dump modal in Schema.
2. Pick dump type + specific tables (pg/mysql).
3. Save dump and optionally download immediately.
4. Use dumps drawer to restore/download/delete later.

### Flow C: CSV ETL
1. Upload CSV.
2. Pick target table.
3. Map CSV headers to DB columns (auto-map assisted).
4. Import in batches with live progress.
5. Download rejects for failed rows.

## 8. Trust, security, and privacy signals you can use

- Self-hosted architecture (data stays in your infra).
- Local persistence for app metadata (`/app/data` volume).
- Native SSH tunnel support for protected networks.
- Path validation present on download/delete dump endpoints.
- Strong explicit error surfacing in most server paths.

## 9. Deployment and ops facts

- Multi-stage Docker build.
- Runtime healthcheck configured.
- Compose examples for app and persistent volume.
- Dev compose includes Postgres + Adminer for local sandboxing.

## 10. UX and brand direction from code

- Terminal/hardware/neobrutalist language:
  - `SYSTEM_PATCHBAY`, `SCHEMA_EXPLORER`, `DATA_INJECTION_MODULE`
- Heavy tactile UI style:
  - `border-2`, `rounded-none`, `shadow-hardware`, uppercase labels, mono data
- “Operator console” tone: direct, technical, command-like actions

## 11. Messaging building blocks (reusable copy primitives)

### Hero angles (pick one)
1. **“Your self-hosted control panel for dumps, schema, and data imports.”**
2. **“Move faster with database ops in one secure developer cockpit.”**
3. **“From connection to restore in minutes — without leaving your infrastructure.”**

### Value proposition bullets
- Manage Postgres, MySQL, and SQLite from one interface.
- Create targeted SQL dumps and restore on demand.
- Import CSV with mapping, progress streaming, and reject auditing.
- Connect through SSH tunnels without exposing database ports.
- Keep credentials and artifacts inside your own environment.

### CTA options
- Primary: **Run with Docker Compose**
- Secondary: **Explore schema and dump workflows**
- Tertiary: **Import CSV with live mapping**

## 12. Guardrails: what not to claim

Do **not** claim these as fully solved without verification:
1. “Selective dump for SQLite” (not native; current fallback dumps all)
2. “Enterprise-grade security certifications” (not present in repo)
3. “Comprehensive automated test coverage” (tests are currently minimal)
4. “Guaranteed production-safe rollback/transactions for all operations”

## 13. Known implementation caveats (important for truthful copy)

1. **Seed API contract mismatch likely exists**  
   `seed.ts` reads query params, while UI sends JSON body to `/api/seed`.
2. **Some roadmap docs are aspirational**  
   Keep marketing claims anchored to code and current UX, not roadmap intent.

## 14. Evidence map (source-of-truth files)

### Product and UX
- `src/routes/__root.tsx`
- `src/routes/index.tsx`
- `src/routes/schema.tsx`
- `src/routes/import.tsx`
- `src/routes/_index/components/*`
- `src/routes/_schema/components/*`
- `src/styles.css`

### Server and data layer
- `src/lib/db/connection.ts`
- `src/server/connection-fns.ts`
- `src/server/saved-connections.ts`
- `src/server/schema-fns.ts`
- `src/server/ssh-tunnel.ts`
- `src/server/internal-db.ts`

### APIs
- `src/routes/api/test-connection.ts`
- `src/routes/api/export-schema.ts`
- `src/routes/api/export-csv.ts`
- `src/routes/api/dump.ts`
- `src/routes/api/download-dump.ts`
- `src/routes/api/import.ts`
- `src/routes/api/download-reject.ts`
- `src/routes/api/seed.ts`

### Deployment and docs
- `Dockerfile`
- `Dockerfile.dev`
- `compose.yml`
- `compose-dev.yml`
- `README.md`
- `ARCHITECTURE.md`
- `INSTRUCTIONS.md`

### Tests
- `tests/sanity.test.ts`
- `tests/use-active-connection.test.tsx`
- `tests/with-tunnel.test.ts`

## 15. Prompt starter for the next LLM

Use this exact starter when generating the landing page:

> Build a conversion-oriented landing page for OmniDump using the capabilities and constraints in `LANDING_PAGE_CONTEXT.md`.  
> Keep claims strictly limited to shipped features, call out self-hosted + SSH + dump/import workflows, and preserve the technical “operator console” tone.  
> Include: hero, feature pillars, workflow section, trust/security section, deployment quick-start CTA, and FAQ.

