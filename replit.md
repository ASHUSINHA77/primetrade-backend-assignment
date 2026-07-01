# TaskFlow — Backend Developer Intern Assignment

A scalable REST API with JWT authentication, role-based access control, and a React frontend — built for the Primetrade.ai Backend Developer Intern assessment.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 8080)
- `pnpm --filter @workspace/web run dev` — run the React frontend (port 22333)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string
- Required env: `SESSION_SECRET` — JWT signing secret

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- API: Express 5 + JWT (jsonwebtoken) + bcrypt
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Docs: Swagger UI at `/api/docs`
- Frontend: React + Vite + TanStack Query + wouter
- Build: esbuild (CJS bundle)

## Where things live

- `lib/api-spec/openapi.yaml` — OpenAPI 3.1 contract (source of truth)
- `lib/db/src/schema/users.ts` — Users table (id, name, email, passwordHash, role, timestamps)
- `lib/db/src/schema/tasks.ts` — Tasks table (id, userId FK, title, description, status, priority, timestamps)
- `artifacts/api-server/src/middlewares/auth.ts` — JWT middleware (authenticate, requireAdmin)
- `artifacts/api-server/src/routes/v1/auth.ts` — Register, login, /me
- `artifacts/api-server/src/routes/v1/tasks.ts` — Task CRUD + stats
- `artifacts/api-server/src/routes/v1/admin.ts` — Admin: users list/delete, platform stats
- `artifacts/web/src/` — React frontend (pages: login, register, dashboard, tasks, admin)
- `lib/api-client-react/src/custom-fetch.ts` — Injects JWT Authorization header

## Architecture decisions

- JWT stored in localStorage (`taskflow_token`); injected via custom-fetch wrapper
- Role-based access enforced on both server (middleware) and client (route guards)
- Users own their tasks; admins can read/delete any task or user
- OpenAPI spec drives both server Zod validation and client React Query hooks via Orval codegen
- Swagger UI served at `/api/docs` from the same OpenAPI YAML file at runtime

## Product

- Register/login with JWT authentication
- Protected dashboard showing task stats (total, pending, in-progress, done, high-priority)
- Full task CRUD with status/priority filtering
- Admin console: user management + platform-wide statistics
- API documentation via Swagger UI

## Demo Accounts

| Role  | Email                  | Password     |
|-------|------------------------|--------------|
| Admin | admin@taskflow.dev     | Admin@123!   |
| User  | jane@taskflow.dev      | User@123!    |

## Scalability Note

This API is designed for horizontal scalability:
- **Stateless JWT auth** — no server-side sessions; any instance can verify tokens
- **Connection pooling** — `pg.Pool` reuses DB connections across requests
- **Modular route structure** — each domain (auth, tasks, admin) is independently deployable as a microservice
- **OpenAPI contract-first** — enables independent frontend/backend team scaling
- **Caching ready** — `/v1/tasks/stats` and `/v1/admin/stats` are read-heavy aggregates, ideal candidates for Redis caching with short TTLs (30–60s)
- **Load balancing ready** — stateless design means requests can be round-robined across N API instances behind a load balancer (Nginx, AWS ALB)

## User preferences

_Populate as you build — explicit user instructions worth remembering across sessions._

## Gotchas

- Run `pnpm --filter @workspace/api-spec run codegen` after any OpenAPI spec change
- Run `pnpm --filter @workspace/db run push` after any schema change
- js-yaml must be imported as `import * as yaml from 'js-yaml'` (no default export in v5 ESM)

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
