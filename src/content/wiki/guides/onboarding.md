---
title: "New Project Onboarding"
description: "Checklist untuk setup project baru di semua stack."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["onboarding", "setup", "checklist"]
isPinned: true
growthStage: "evergreen"
---

## Checklist Project Baru

### 1. Repository Setup

- [ ] Buat repo di GitHub
- [ ] Setup `.gitignore` sesuai stack
- [ ] Add `README.md` dengan deskripsi, install, run, test
- [ ] Add `LICENSE` (MIT untuk personal/open source)
- [ ] Setup branch protection untuk `main` dan `dev`

### 2. Tooling

- [ ] Setup [[git-hooks\|Lefthook]] (`lefthook.yml`)
- [ ] Setup [[commit-convention\|commitlint]] (`commitlint.config.mjs`)
- [ ] Setup linter + formatter ([[linting-formatting\|ESLint/Prettier/Biome]])
- [ ] Setup TypeScript config (`tsconfig.json`)

### 3. Environment

- [ ] Buat `.env.example` dengan semua required variables
- [ ] Jangan commit `.env` yang real
- [ ] Dokumentasikan cara setup DB, Redis, dll

### 4. Database

- [ ] Pilih ORM/query builder sesuai stack
- [ ] Buat migration awal
- [ ] Setup seed data untuk development
- [ ] Document schema di README

### 5. API / Backend

- [ ] Setup health check endpoint (`/health`)
- [ ] Setup error handling global
- [ ] Setup logging (structured JSON)
- [ ] Setup rate limiting

### 6. Frontend

- [ ] Setup Tailwind CSS
- [ ] Setup design tokens / theme
- [ ] Setup layout base (header, footer, nav)
- [ ] Setup error boundary / fallback UI

### 7. Testing

- [ ] Setup test runner (Vitest / Jest / go test)
- [ ] Buat 1 unit test sebagai contoh
- [ ] Setup CI pipeline (GitHub Actions)

### 8. CI/CD

- [ ] Setup [[ci-cd\|GitHub Actions]] workflow
- [ ] Lint + type check + test di setiap PR
- [ ] Setup deployment ke staging/production

### 9. Dokumentasi

- [ ] Update wiki dengan arsitektur project
- [ ] Document API endpoints (OpenAPI/Postman)
- [ ] Add ADR (Architecture Decision Record) jika perlu

## Per-Stack Quick Start

### Go

```bash
# Clone boilerplate
git clone https://github.com/khoirul/gokit-starter my-project
cd my-project

# Setup
cp .env.example .env
go mod tidy

# Run
go run ./cmd/server

# Test
go test ./...
```

### Astro

```bash
# New project
pnpm create astro@latest my-project

# Setup tools
pnpm add -D lefthook @commitlint/cli @commitlint/config-conventional

# Config files
# - lefthook.yml
# - commitlint.config.mjs
# - eslint.config.js
# - .prettierrc
```

### Laravel

```bash
# New project
composer create-project laravel/laravel my-project

# Setup
cp .env.example .env
php artisan key:generate
php artisan migrate

# Dev
php artisan serve
```

### SvelteKit

```bash
# New project
npx sv create my-project

# Setup
cd my-project
pnpm install
pnpm add -D tailwindcss @tailwindcss/vite
```

pnpm dev

```

```
