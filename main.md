# CivicConnect

CivicConnect helps Hyderabad residents report civic issues, follow resolution progress, and join neighborhood activities.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 5000)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `artifacts/civicconnect/` — responsive citizen and authority web app
- `artifacts/api-server/src/routes/civic.ts` — dashboard, issue, and activity API routes
- `lib/api-spec/openapi.yaml` — source of truth for API contracts
- `lib/db/src/schema/` — Drizzle tables for civic issues and activities
- `artifacts/civicconnect/src/index.css` — CivicConnect visual tokens and global styles

## Architecture decisions

- The first release is a responsive web app rather than a native mobile app, so the core reporting-to-resolution loop can be validated quickly across desktop and mobile widths.
- AI classification is represented as confidence and priority fields in the API contract, keeping the UI ready for a real inference service without coupling the first release to a model provider.
- Civic issue photos use an optional URL field in the first release; persistent file uploads should be added through App Storage before accepting production image bytes.

## Product

- Citizens can see a neighborhood dashboard, browse nearby issue reports, support issues, submit new reports, and join community activities.
- Authorities can use the workspace view to filter issues, review AI confidence and priority, and update resolution status.
- Seeded Miyapur/Hyderabad data keeps the experience useful on first load while new reports and activity joins persist to PostgreSQL.

## User preferences

_Populate as you build — explicit user instructions worth remembering across sessions._

## Gotchas

- Run `pnpm --filter @workspace/api-spec run codegen` after changing `lib/api-spec/openapi.yaml`.
- The generated Zod package currently resolves to Zod 3, so OpenAPI numeric fields use `type: number` rather than `type: integer` to avoid generated `z.int()` calls.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
