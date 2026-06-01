# Next.js Architecture: PWA / Local-First (Shared Domain)

Use when the app works offline and syncs data locally via IndexedDB.

**Dependency rule**: `presentation → client → domain ← server`

- `domain/` at project root — shared by both sides, zero external dependencies
- `shared/` — DTOs, pure utils, constants, hooks, PWA config
- `server/` — server application layer + Prisma infrastructure
- `client/` — client application layer + local persistence (Dexie)
- `presentation/` — React UI only, no business logic
- `app/` — Next.js App Router routing only

Repository interfaces live at each side's application boundary (not in domain), because server and client have different contracts (Prisma vs Dexie).

---

## Folder structure

```
domain/
  entities/
  value-objects/
  services/
    __tests__/

server/
  application/
    repositories/        ← Prisma-facing interfaces
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
    repositories/        ← Dexie-facing interfaces
    services/
      __tests__/
  infrastructure/
    http/                ← API client implementation
    dexie/               ← db.ts + Dexie repository implementations
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
  pwa/                   ← serwist config (only if serwist installed)
  i18n/                  ← next-intl config (only if next-intl installed)

prisma/                  ← created by `prisma init` in Step 4
  schema.prisma
  migrations/
  generated/
    prisma/
```

## Creation commands

```bash
mkdir -p domain/entities domain/value-objects domain/services/__tests__
mkdir -p server/application/repositories
mkdir -p server/infrastructure/http server/infrastructure/prisma-orm server/infrastructure/seeds/data
mkdir -p client/application/api client/application/repositories client/application/services/__tests__
mkdir -p client/infrastructure/http client/infrastructure/dexie
mkdir -p client/stores
mkdir -p presentation/web/pages presentation/web/components
mkdir -p presentation/components/ui
mkdir -p "app/(web)" app/api
mkdir -p shared/dtos shared/lib shared/config shared/hooks
# conditional:
mkdir -p shared/pwa       # if serwist
mkdir -p shared/i18n      # if next-intl
```

Add `.gitkeep` to empty leaf directories.
