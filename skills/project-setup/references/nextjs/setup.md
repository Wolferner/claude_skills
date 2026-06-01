# Next.js Setup Guide

**IMPORTANT**: This project uses Next.js 16 which has breaking changes.
After initializing, read `node_modules/next/dist/docs/` before writing any Next.js-specific code.

---

## Step 1: Gather all requirements

Ask both questions before doing anything else.

**Architecture type:**

**PWA / local-first** — app works offline, data synced to IndexedDB locally:
- `domain/` at project root, shared between server and client
- Client has its own application layer with local repositories

**Standard fullstack** — server is the authority, client is thin:
- `domain/` lives inside `server/`
- Client is a simple API consumer, no local persistence

**Optional modules** (defaults marked — just confirm):

| Module | Default | Notes |
|---|---|---|
| Prisma + Neon (PostgreSQL) | **yes** | DB layer |
| Zustand | **yes** | Client state management |
| shadcn/ui | **yes** | Component library (radix-ui) |
| serwist | only if PWA | Service worker, offline support |
| Dexie | only if PWA | IndexedDB wrapper |
| next-intl | **no** | i18n — only if explicitly needed |

---

## Step 2: Initialize

```bash
npx create-next-app@latest <project-name> \
  --typescript \
  --eslint \
  --tailwind \
  --app \
  --no-src-dir \
  --import-alias "@/*"
cd <project-name>
```

---

## Step 3: Install dependencies

### Always

```bash
npm install zod
npm install -D eslint-config-prettier
```

### Prisma + Neon

```bash
npm install @prisma/client @prisma/adapter-neon
npm install -D prisma
npx prisma init --datasource-provider postgresql
```

Update `prisma/schema.prisma` generator block:
```prisma
generator client {
  provider = "prisma-client-js"
  output   = "../prisma/generated/prisma"
}
```

Add to `package.json` scripts: `"postinstall": "npx prisma generate"`.

### Zustand

```bash
npm install zustand
```

### shadcn/ui

```bash
npx shadcn@latest init
```
Answer: style = Default, base color = Neutral, CSS variables = yes.

### serwist (PWA only)

```bash
npm install serwist
npm install -D @serwist/turbopack
```

### Dexie (PWA only)

```bash
npm install dexie dexie-react-hooks
```

### next-intl

```bash
npm install next-intl
```

---

## Step 4: Folder structure

Read the corresponding arch file and execute the creation commands there:
- PWA → `arch-pwa.md`
- Standard → `arch-standard.md`

The arch file has conditional directories for optional modules — create only the ones that match the selected modules.

---

## Step 5: Next.js-specific config

### `eslint.config.mjs` — replace the generated file

```js
import nextVitals from 'eslint-config-next/core-web-vitals';
import nextTs from 'eslint-config-next/typescript';
import prettier from 'eslint-config-prettier/flat';
import { defineConfig, globalIgnores } from 'eslint/config';

const eslintConfig = defineConfig([
  ...nextVitals,
  ...nextTs,
  prettier,
  {
    rules: {
      'no-console': ['warn'],
      'no-inner-declarations': ['error', 'functions'],
      'prefer-arrow-callback': 'error',
      'prefer-template': ['warn'],
    },
  },
  globalIgnores(['.next/**', 'out/**', 'build/**', 'next-env.d.ts']),
]);

export default eslintConfig;
```

### `next.config.ts`

No optional modules:
```ts
import type { NextConfig } from 'next';
const nextConfig: NextConfig = {};
export default nextConfig;
```

With serwist + next-intl (include only installed wrappers):
```ts
import { withSerwist } from '@serwist/turbopack';
import createNextIntlPlugin from 'next-intl/plugin';
import type { NextConfig } from 'next';

const withNextIntl = createNextIntlPlugin('./shared/i18n/request.ts');
const nextConfig: NextConfig = {};
export default withNextIntl(withSerwist(nextConfig));
```

### `package.json` — Next.js scripts

```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
  }
}
```

---

## Step 6: Common tooling

Read and follow `../common/tooling.md` — covers Prettier (add `prettier-plugin-tailwindcss`), Husky + lint-staged, Vitest (use `jsdom` environment), git, README/CHANGELOG/LICENSE.

---

## Step 7: Verify

### Tooling

```bash
npm run typecheck && npm run lint:check && npm run format:check && npm run test
```

### Folder structure

Compare the created directories against the chosen arch file. Check that every folder in the spec exists and no required folder is missing.

```bash
find . -type d -not -path '*/node_modules/*' -not -path '*/.next/*' -not -path '*/.git/*' | sort
```

### package.json

Check that all required fields are present:
- `name`, `version`, `private`, `type: "module"`
- `repository.url` with correct project name
- `keywords` (non-empty array)
- `author` with name and email
- `scripts`: `dev`, `build`, `start`, `lint`, `lint:check`, `format:check`, `format:write`, `typecheck`, `test`, `prepare`, `ci:check`
- `postinstall` (if Prisma is installed)
- `lint-staged` config

Check that all selected modules appear in `dependencies` or `devDependencies`.

Fix everything before finishing.
