# Next.js Architecture: Standard Fullstack

Use when the server is the authority and the client is a thin consumer — no offline support, no local persistence.

**Dependency rule**: `presentation → client → server`

- `server/` — domain + application layer + Prisma infrastructure
- `shared/` — DTOs (the API contract between server and client)
- `client/` — thin API consumer, no local repositories
- `presentation/` — React UI only, no business logic
- `app/` — Next.js App Router routing only

Domain stays server-side — no reason to share it with the client.

---

## Folder structure

```
server/
  domain/
    entities/
    value-objects/
    services/
      __tests__/
  application/
    repositories/        ← interfaces
    [feature]/           ← use cases per feature
      __tests__/
  infrastructure/
    http/                ← controllers, DI container
    prisma-orm/          ← Prisma repos, prismaClient.ts
    seeds/
      data/

client/
  application/
    api/                 ← API-facing interfaces
    services/
  infrastructure/
    http/                ← API client implementation
  stores/                ← Zustand stores

presentation/
  web/
    pages/
    components/
  components/
    ui/                  ← shadcn components

app/
  (web)/
    page.tsx
  api/
    [resource]/
      route.ts

shared/
  dtos/                  ← API contracts (request/response DTOs)
  lib/                   ← pure utils (cn(), etc.)
  config/                ← constants
  hooks/                 ← shared React hooks
  i18n/                  ← next-intl config (only if next-intl installed)

prisma/                  ← created by `prisma init` in Step 4
  schema.prisma
  migrations/
  generated/
    prisma/
```

## Creation commands

```bash
mkdir -p server/domain/entities server/domain/value-objects server/domain/services/__tests__
mkdir -p server/application/repositories
mkdir -p server/infrastructure/http server/infrastructure/prisma-orm server/infrastructure/seeds/data
mkdir -p client/application/api client/application/services
mkdir -p client/infrastructure/http
mkdir -p client/stores
mkdir -p presentation/web/pages presentation/web/components
mkdir -p presentation/components/ui
mkdir -p "app/(web)" app/api
mkdir -p shared/dtos shared/lib shared/config shared/hooks
# conditional:
mkdir -p shared/i18n      # if next-intl
```

Add `.gitkeep` to empty leaf directories.
