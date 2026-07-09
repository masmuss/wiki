---
title: "Local Environment Setup"
description: "Prasyarat dan toolchain yang digunakan di semua project."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["setup", "toolchain", "environment"]
isPinned: true
growthStage: "evergreen"
---

## Package Manager

Semua project JavaScript/TypeScript menggunakan **pnpm**.
Alasan: lebih cepat, disk-efficient, dan strict dependency resolution.

```bash
corepack enable
corepack prepare pnpm@latest --activate
```

Project Go menggunakan **Go modules** bawaan.
Project PHP/Laravel menggunakan **Composer**.

## Runtime Requirements

| Runtime | Versi      | Method       |
| ------- | ---------- | ------------ |
| Node.js | >= 22.12.0 | fnm / nvm    |
| Go      | >= 1.22    | go install   |
| PHP     | >= 8.2     | homebrew     |
| Bun     | latest     | curl install |

```bash
# Node.js via fnm (recommended)
fnm install 22
fnm default 22

# Go
brew install go

# PHP + extensions
brew install php composer

# Bun
curl -fsSL https://bun.sh/install | bash
```

## Global Tools

```bash
# Core
pnpm add -g typescript @antfu/ni

# Go tools
go install github.com/go-delve/delve/cmd/dlv@latest
go install golang.org/x/tools/cmd/goimports@latest
go install github.com/air-verse/air@latest
```

## Verify Setup

```bash
pnpm --version
go version
php --version
bun --version
```
