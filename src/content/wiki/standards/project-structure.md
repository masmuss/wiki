---
title: "Project Structure Standards"
description: "Standarisasi struktur direktori untuk berbagai jenis project."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["standards", "structure"]
isPinned: false
growthStage: "evergreen"
---

## Root File Conventions

Semua project Wajib memiliki:

```
├── .gitignore
├── README.md
├── LICENSE (opsional)
└── Makefile (atau just-task, taskfile)
```

## Monorepo Structure

Untuk project yang terdiri dari multiple packages:

```
├── apps/
│   ├── web/          # Frontend (Astro/Next/SvelteKit)
│   └── api/          # Backend (Go/Laravel/NestJS/Hono)
├── packages/
│   ├── shared/       # Shared types, utils
│   └── ui/           # Shared UI components
├── tooling/          # ESLint, Prettier, TS configs
├── pnpm-workspace.yaml
└── package.json
```

## README Wajib Berisi

1. **Judul** dan deskripsi singkat
2. **Prerequisites** (runtime, tools)
3. **Cara install** (`pnpm install`, `go mod download`, dll)
4. **Cara jalanin** (`pnpm dev`, `go run .`, dll)
5. **Testing** (`pnpm test`, `go test ./...`, dll)
6. **Environment variables** (copy `.env.example`)
7. **Database setup** jika ada
8. **Deployment** (jika relevant)
