# Node.js Setup Guide

> Stub — expand with full config details when setting up the first Node.js project.

---

## Step 1: Gather all requirements

Ask before doing anything else.

**Optional modules** (defaults marked — just confirm):

| Module | Default | Notes |
|---|---|---|
| Fastify | **yes** | HTTP framework (alternative: Express) |
| Prisma + PostgreSQL | **yes** | DB layer |
| Zod | **yes** | Validation |

No frontend modules (Zustand, shadcn, serwist, Dexie) — this is server-only.

---

## Step 2: Initialize

```bash
npm init -y
npm install -D typescript tsx @types/node
npx tsc --init
```

---

## Step 3: Install dependencies

### Always

```bash
npm install zod
npm install -D eslint-config-prettier @typescript-eslint/eslint-plugin @typescript-eslint/parser
```

### Fastify

```bash
npm install fastify
```

### Prisma

```bash
npm install @prisma/client
npm install -D prisma
npx prisma init --datasource-provider postgresql
```

Add to `package.json` scripts: `"postinstall": "npx prisma generate"`.

---

## Step 4: Folder structure

```
src/
  domain/
    entities/
    value-objects/
    services/
      __tests__/
  application/
    repositories/
    [feature]/
      __tests__/
  infrastructure/
    http/
    prisma-orm/
    seeds/
      data/
```

```bash
mkdir -p src/domain/entities src/domain/value-objects src/domain/services/__tests__
mkdir -p src/application/repositories
mkdir -p src/infrastructure/http src/infrastructure/prisma-orm src/infrastructure/seeds/data
```

Add `.gitkeep` to empty leaf directories.

---

## Step 5: Node.js-specific config

### `eslint.config.mjs`

> Expand with full config when setting up first project.

Use `@typescript-eslint/eslint-plugin` + `eslint-config-prettier`. No `eslint-config-next`.

### `package.json` — Node.js scripts

```json
{
  "scripts": {
    "dev": "tsx watch src/index.ts",
    "build": "tsc",
    "start": "node dist/index.js"
  }
}
```

---

## Step 6: Common tooling

Read and follow `../common/tooling.md` — covers package.json metadata, Prettier, Husky + lint-staged, Vitest (use `node` environment, no jsdom or `@testing-library`), git, README/CHANGELOG/LICENSE.

---

## Step 7: Verify

### Tooling

```bash
npm run typecheck && npm run lint:check && npm run format:check && npm run test
```

### Folder structure

```bash
find . -type d -not -path '*/node_modules/*' -not -path '*/.git/*' | sort
```

### package.json

Check that all required fields are present:
- `name`, `version`, `private`, `type: "module"`
- `repository.url`, `keywords`, `author`
- `scripts`: `dev`, `build`, `start`, `lint`, `lint:check`, `format:check`, `format:write`, `typecheck`, `test`, `prepare`, `ci:check`
- `postinstall` (if Prisma installed)
- `lint-staged` config

Check that all selected modules appear in `dependencies` or `devDependencies`.

Fix everything before finishing.
