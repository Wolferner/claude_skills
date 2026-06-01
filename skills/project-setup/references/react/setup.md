# React SPA Setup Guide

> Stub — expand with full config details when setting up the first React SPA project.

---

## Step 1: Gather all requirements

Ask before doing anything else.

**Optional modules** (defaults marked — just confirm):

| Module | Default | Notes |
|---|---|---|
| Zustand | **yes** | State management |
| shadcn/ui | **yes** | Component library (radix-ui) |
| React Router | **yes** | Client-side routing |
| Zod | **yes** | Validation |
| Tailwind CSS | **yes** | Included via shadcn, confirm separately if no shadcn |

No server modules (Prisma, serwist, Dexie) — this is frontend-only.

---

## Step 2: Initialize

```bash
npm create vite@latest <project-name> -- --template react-ts
cd <project-name>
npm install
```

---

## Step 3: Install dependencies

### Always

```bash
npm install zod
npm install -D eslint-config-prettier
```

### Zustand

```bash
npm install zustand
```

### React Router

```bash
npm install react-router-dom
```

### shadcn/ui

```bash
npx shadcn@latest init
```
Answer: style = Default, base color = Neutral, CSS variables = yes.

---

## Step 4: Folder structure

```
src/
  application/
    api/
    services/
      __tests__/
  infrastructure/
    http/
  stores/              ← Zustand
  presentation/
    pages/
    components/
      ui/              ← shadcn
  shared/
    lib/
    config/
    hooks/
```

```bash
mkdir -p src/application/api src/application/services/__tests__
mkdir -p src/infrastructure/http
mkdir -p src/stores
mkdir -p src/presentation/pages src/presentation/components/ui
mkdir -p src/shared/lib src/shared/config src/shared/hooks
```

Add `.gitkeep` to empty leaf directories.

---

## Step 5: React-specific config

### `eslint.config.mjs`

> Expand with full config when setting up first project.

Use `eslint-plugin-react` + `@typescript-eslint/eslint-plugin` + `eslint-config-prettier`. No `eslint-config-next`.

### `package.json` — Vite scripts

```json
{
  "scripts": {
    "dev": "vite",
    "build": "tsc -b && vite build",
    "preview": "vite preview"
  }
}
```

---

## Step 6: Common tooling

Read and follow `../common/tooling.md` — covers package.json metadata, Prettier (add `prettier-plugin-tailwindcss` if Tailwind installed), Husky + lint-staged, Vitest (use `jsdom` environment), git, README/CHANGELOG/LICENSE.

---

## Step 7: Verify

### Tooling

```bash
npm run typecheck && npm run lint:check && npm run format:check && npm run test
```

### Folder structure

```bash
find . -type d -not -path '*/node_modules/*' -not -path '*/.git/*' -not -path '*/dist/*' | sort
```

### package.json

Check that all required fields are present:
- `name`, `version`, `private`, `type: "module"`
- `repository.url`, `keywords`, `author`
- `scripts`: `dev`, `build`, `preview`, `lint`, `lint:check`, `format:check`, `format:write`, `typecheck`, `test`, `prepare`, `ci:check`
- `lint-staged` config

Check that all selected modules appear in `dependencies` or `devDependencies`.

Fix everything before finishing.
