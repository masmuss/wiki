---
title: "Linting & Formatting"
description: "Standarisasi linting dan formatting untuk semua bahasa."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["lint", "format", "eslint", "prettier", "biome"]
isPinned: false
growthStage: "evergreen"
---

## JavaScript / TypeScript / Astro

### ESLint

Config: `eslint.config.js`

```js
import eslintPluginAstro from "eslint-plugin-astro";

export default [
  ...eslintPluginAstro.configs["flat/recommended"],
  {
    ignores: ["dist/", ".astro/", "node_modules/"],
  },
];
```

Per-project bisa ditambah plugin sesuai stack (React, Svelte, dll).

Script `package.json`:

```json
{
  "scripts": {
    "lint": "eslint .",
    "lint:fix": "eslint . --fix"
  }
}
```

### Prettier

Config: `.prettierrc`

```json
{
  "plugins": ["prettier-plugin-astro"],
  "overrides": [
    {
      "files": "*.astro",
      "options": {
        "parser": "astro"
      }
    }
  ]
}
```

### Biome (Zed Editor)

Untuk project yang pure TypeScript/JavaScript, bisa ganti ke Biome yang lebih cepat.
Biome handle linting + formatting sekaligus.

Script `package.json`:

```json
{
  "scripts": {
    "lint": "biome check .",
    "format": "biome format --write .",
    "format:check": "biome check --formatter-enabled=true"
  }
}
```

## Go

Gunakan tooling bawaan Go:

```bash
# Formatting
gofmt -l -w .

# Linting (standalone)
go vet ./...

# Linting (comprehensive) — install golangci-lint
golangci-lint run ./...
```

## PHP / Laravel

```bash
# Formatting
./vendor/bin/pint

# Linting (Laravel)
./vendor/bin/phpstan analyse --memory-limit=2G
```
