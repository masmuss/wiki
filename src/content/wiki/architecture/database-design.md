---
title: "Database Design"
description: "Prinsip desain database untuk PostgreSQL."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["architecture", "database", "postgresql"]
isPinned: false
growthStage: "evergreen"
---

## Preferred DB: PostgreSQL

Default untuk semua project backend (Go, Laravel, NestJS, Bun/Hono).

## Schema Design

1. **Use snake_case** untuk nama tabel dan kolom.
2. **Tabel plural**: `users`, `posts`, `orders`.
3. **Foreign key naming**: `user_id`, `post_id`.
4. **Timestamps**: `created_at`, `updated_at`, `deleted_at` (soft delete jika perlu).
5. **UUID primary key** untuk distributed systems, **SERIAL/BIGINT** untuk single-node.

## Migration Strategy

Gunakan migration tool sesuai stack:

| Stack   | Tool                |
| ------- | ------------------- |
| Go      | Atlas / goose       |
| Laravel | Eloquent migrations |
| NestJS  | TypeORM / Prisma    |
| Bun     | Drizzle ORM         |

## Indexing

- Index kolom yang sering di-`WHERE`, `JOIN`, `ORDER BY`.
- Pertimbangkan **partial index** untuk query dengan `WHERE status = 'active'`.
- Cek dengan `EXPLAIN ANALYZE` sebelum deploy.

## Soft Delete

Pakai `deleted_at timestamp NULL DEFAULT NULL`.
Query default pakai `WHERE deleted_at IS NULL`.

## Connection Pool

- Go: `SetMaxOpenConns(25)`, `SetMaxIdleConns(10)`.
- NestJS/Prisma: Default pool cukup, adjust berdasarkan load.
