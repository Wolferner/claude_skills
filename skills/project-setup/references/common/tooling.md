# Common Tooling

Applies to all frameworks. Run after framework-specific initialization.

---

## package.json

Ask the user for the project name and keywords if not already known. Set metadata and common scripts together:

```json
{
  "name": "<project-name>",
  "version": "0.1.0",
  "private": true,
  "type": "module",
  "repository": {
    "type": "git",
    "url": "https://github.com/Wolferner/<project-name>.git"
  },
  "keywords": [],
  "author": {
    "name": "Wolferner",
    "email": "maksim.scerbuk@gmail.com"
  },
  "scripts": {
    "lint": "eslint",
    "lint:check": "npx eslint .",
    "format:check": "prettier --check .",
    "format:write": "prettier --write .",
    "typecheck": "tsc --noEmit",
    "test": "vitest run",
    "prepare": "husky",
    "ci:check": "npm run typecheck && npm run format:check && npm run lint:check && npm run test"
  }
}
```

Framework-specific scripts (dev, build, start, lint) are added in each framework's setup guide.

---

## Git

```bash
git init
git add .
git commit -m "chore: initial commit"
```

Create `.gitignore` if the framework didn't generate one. Always exclude:
```
node_modules/
.env
.env.local
dist/
build/
```

---

## Prettier

Install:
```bash
npm install -D prettier
```

`.prettierrc`:
```json
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5"
}
```

If the project uses Tailwind CSS, also install `prettier-plugin-tailwindcss` and add it to plugins:
```bash
npm install -D prettier-plugin-tailwindcss
```
```json
{
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

---

## Husky + lint-staged

```bash
npm install -D husky lint-staged
npx husky init
```

Replace `.husky/pre-commit`:
```sh
#!/usr/bin/env sh
echo "→ pre-commit: lint-staged"
npx lint-staged
```

Add to `package.json`:
```json
{
  "lint-staged": {
    "*.{ts,tsx,js,jsx}": ["eslint --fix", "prettier --write"],
    "*.{md,json,yml,yaml,css,scss}": ["prettier --write"]
  }
}
```

---

## Vitest

```bash
npm install -D vitest @vitest/coverage-v8 @testing-library/jest-dom jsdom
```

For projects with React UI, also install:
```bash
npm install -D @testing-library/react @testing-library/user-event
```

`vitest.config.ts`:
```ts
import { defineConfig } from 'vitest/config';
import path from 'path';

export default defineConfig({
  test: {
    globals: true,
    environment: 'jsdom',   // use 'node' for server-only projects
    setupFiles: ['./test/setup.ts'],
  },
  resolve: {
    alias: { '@': path.resolve(__dirname, '.') },
  },
});
```

`test/setup.ts`:
```ts
import '@testing-library/jest-dom';  // omit for server-only projects
```

---

## .env.example

Create `.env.example` at the root. Include only the variables relevant to the selected modules — never include actual values, only placeholder descriptions.

| Module | Variable | Placeholder |
|---|---|---|
| Prisma + Neon | `DATABASE_URL` | `postgresql://user:password@host/dbname?sslmode=require` |
| Next.js (general) | `NEXT_PUBLIC_APP_URL` | `http://localhost:3000` |
| Node.js API | `PORT` | `3000` |
| Node.js API | `HOST` | `0.0.0.0` |
| serwist / PWA | `NEXT_PUBLIC_APP_URL` | `http://localhost:3000` |

Also add allowed origins if the project has a proxy or CORS config:
```
ALLOWED_ORIGINS=http://localhost:3000
```

Add `.env` and `.env.local` to `.gitignore`. Never commit actual env files.

---

## Project files

Create these at the root if they don't exist:

**`README.md`** — generate from project context (name, stack, architecture). Must cover everything a new developer needs to get started:

- **What it is** — one paragraph: what the app does and who it's for
- **Stack** — list of key technologies with versions (framework, DB, state management, UI library, etc.)
- **Architecture** — brief description of the layer structure (e.g. Onion: domain / server / client / presentation) and the dependency rule
- **Prerequisites** — Node version, required env vars with descriptions (never values), external services (DB, etc.)
- **Getting started** — exact commands from clone to running locally (`git clone`, `npm install`, `cp .env.example .env`, `npm run dev`)
- **Scripts** — table of all `package.json` scripts and what they do
- **Project structure** — top-level folder tree with one-line description per folder

**`CHANGELOG.md`** — follow [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) format with [Semantic Versioning](https://semver.org/spec/v2.0.0.html):

```md
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.1.0] - YYYY-MM-DD

### Added

- Initial project setup.
```

Sections per release: `Added`, `Changed`, `Fixed`, `Removed`. Only include sections that have entries.

**`LICENSE.md`** — always proprietary. Create with this exact content, substituting the current year:

```md
# PROPRIETARY LICENSE AGREEMENT

Copyright © <year> Maksim Scerbuk. All rights reserved.

## 1. Definitions

"**Software**" means the source code, object code, documentation, and any accompanying files delivered by the **Licensor**.  
"**Licensor**" means Maksim Scerbuk.  
"**Licensee**" means the individual or legal entity exercising the rights granted under this Agreement.

## 2. Grant of License

Subject to the terms hereof, the Licensor grants the Licensee a limited, non‑exclusive, non‑transferable, **revocable** license to use, view, and modify the Software **solely** for the purpose expressly authorized in a written agreement between the parties (the "Authorized Purpose"). No other rights are granted.

## 3. Restrictions

The Licensee shall **not**, directly or indirectly:  
a. copy, publish, distribute, sublicense, sell, lease, or otherwise transfer the Software or any derivative works;  
b. use the Software in any other project or for any purpose other than the Authorized Purpose;  
c. disclose, make available, or provide access to the Software or any portion thereof to any third party;  
d. remove or alter any proprietary notices.

## 4. Ownership

The Software and all intellectual‑property rights therein remain the exclusive property of the Licensor. Any modifications or derivative works created by the Licensee are deemed part of the Software and are hereby assigned to the Licensor.

## 5. Term and Termination

This Agreement is effective until terminated. The Licensor may terminate it immediately upon written notice if the Licensee breaches any provision. Upon termination, the Licensee must cease all use of the Software and destroy all copies in its possession or control.

## 6. Disclaimer of Warranty

THE SOFTWARE IS PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE, AND NON‑INFRINGEMENT.

## 7. Limitation of Liability

IN NO EVENT SHALL THE LICENSOR BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES, LOSS OF USE, DATA, OR PROFITS, OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THE SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

## 8. Governing Law and Jurisdiction

This Agreement shall be governed by and construed in accordance with the laws of the Republic of Latvia, without regard to its conflict‑of‑law principles. Any dispute shall be submitted to the exclusive jurisdiction of the courts located in Riga, Latvia.

## 9. Severability

If any provision of this Agreement is held to be unenforceable, the remaining provisions shall remain in full force and effect.

## 10. Entire Agreement

This Agreement constitutes the entire agreement between the parties with respect to the Software and supersedes all prior or contemporaneous understandings.

---

By accessing or using the Software, the Licensee acknowledges that it has read, understood, and agrees to be bound by the terms of this Agreement.
```
