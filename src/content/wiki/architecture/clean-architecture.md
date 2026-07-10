---
title: "Clean Architecture"
description: "Prinsip clean architecture untuk semua stack backend."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["architecture", "clean-architecture"]
isPinned: false
growthStage: "evergreen"
---

## Prinsip Dasar

1. **Dependency Rule**: Dependensi inner layer di-load dari outer layer, bukan sebaliknya.
2. **Separation of Concerns**: Tiap layer punya tanggung jawab jelas.
3. **Domain Purity**: Core logic bebas dari framework, DB, UI.

## Layer

### 1. Domain Layer (Core)

- **Entity**: Domain model dan business rules.
- **Repository Interface**: Definisikan cara akses data, tanpa implementasi.
- **Use Case**: Business logic murni, bebas framework.

### 2. Application Layer

- **Service**: Implementasi use case.
- **DTO**: Data transfer antar layer.
- **Interface / Port**: Definisi service yang bisa di-inject.

### 3. Infrastructure Layer

- **Repository**: Implementasi konkret (DB, API, cache).
- **Controller / Handler**: HTTP handler, CLI, queue worker.
- **Framework**: NestJS, Laravel, Chi, Hono, dll.

## Dependency Flow

```
Infrastructure → Application → Domain
```

Domain tidak boleh tahu detail framework. Infrastructure bergantung ke Application dan Domain.

## Struktur Go

```
cmd/
internal/
  domain/         # Entity, use case interface
  application/    # Service implementation
  infrastructure/ # DB repo, HTTP handler, middleware
```

## Struktur Laravel

```
app/
  Domain/         # Entity, repository interface
  Application/    # Service, DTO
  Infrastructure/ # Eloquent repo, Controller
```

## Patokan

- Core logic harus bisa di-test tanpa DB atau framework.
- Ganti DB (MySQL → PostgreSQL) hanya perlu ganti repo di infrastructure.
