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
5. **Primary Key**: Default gunakan format **Prefixed NanoID/ULID** sebagai `VARCHAR` (contoh: `usr_2v6d8x1`) yang diekspos sebagai string di API. Jika internal menggunakan `SERIAL/BIGINT`, jangan diekspos ke public API (gunakan kolom UUID eksternal).

## Panduan Tipe Data

| Data         | PostgreSQL Type | Kapan Digunakan                                                                                                      |
| ------------ | --------------- | -------------------------------------------------------------------------------------------------------------------- |
| ID / PK      | `VARCHAR(32)`   | Untuk NanoID/ULID/Prefixed ID (cth: `usr_xyz`)                                                                       |
| String       | `VARCHAR(255)`  | Email, Nama, Title, dll                                                                                              |
| Teks         | `TEXT`          | Deskripsi, Artikel, JSON string fallback                                                                             |
| Uang / Saldo | `DECIMAL(15,2)` | Harga, saldo (hindari tipe `FLOAT` untuk uang)                                                                       |
| Waktu        | `TIMESTAMPTZ`   | **Selalu** gunakan _timezone-aware_ (contoh: `created_at`)                                                           |
| Dinamis      | `JSONB`         | Metadata, seting user, array data tanpa struktur pasti                                                               |
| Enum         | `VARCHAR`       | Hindari native `ENUM` PostgreSQL (sulit di-alter). Gunakan `VARCHAR` dengan `CHECK` constraint atau tabel referensi. |

## Contoh Skema SQL Terstruktur

```sql
CREATE TABLE users (
    id VARCHAR(32) PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    status VARCHAR(50) DEFAULT 'active' CHECK (status IN ('active', 'inactive', 'banned')),
    metadata JSONB DEFAULT '{}'::jsonb,
    created_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP,
    deleted_at TIMESTAMPTZ NULL
);

CREATE TABLE orders (
    id VARCHAR(32) PRIMARY KEY,
    user_id VARCHAR(32) NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    total_amount DECIMAL(15,2) NOT NULL,
    created_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP
);
```

## Migration Strategy

Gunakan migration tool sesuai stack:

| Stack   | Tool                |
| ------- | ------------------- |
| Go      | Atlas / goose       |
| Laravel | Eloquent migrations |
| NestJS  | TypeORM / Prisma    |
| Bun     | Drizzle ORM         |

### Contoh Penamaan File Migrasi

Gunakan format prefix timestamp untuk menghindari _conflict_:

- `20260710123000_create_users_table.sql` (Raw SQL / goose)
- `2026_07_10_123000_create_users_table.php` (Laravel)

### Contoh Isi File Migrasi (Goose/SQL)

```sql
-- +goose Up
CREATE TABLE posts (
    id VARCHAR(32) PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    body TEXT
);

-- +goose Down
DROP TABLE posts;
```

## Indexing

- **B-Tree (Default)**: Untuk pencarian exact match, inequality (`>`, `<`), dan sorting. Selalu index kolom Foreign Key dan kolom yang sering di-`ORDER BY`.
  - Contoh: `CREATE INDEX idx_orders_user_id ON orders(user_id);`
- **GIN Index**: Wajib untuk pencarian _full-text search_ atau mencari key di dalam kolom `JSONB`.
  - Contoh: `CREATE INDEX idx_users_metadata ON users USING GIN (metadata);`
- **Partial Index**: Sangat disarankan untuk memfilter data aktif guna menghemat memori.
  - Contoh: `CREATE INDEX idx_users_active_email ON users(email) WHERE deleted_at IS NULL;`
- **Composite Index**: Jika sering filter berdasarkan 2 kolom bersamaan (`WHERE category_id = 1 AND status = 'active'`). Urutan kolom di index itu penting (Left-most prefix rule).
- **Hindari Over-indexing**: Setiap index memperlambat proses `INSERT` dan `UPDATE`.
- Selalu cek dengan `EXPLAIN ANALYZE` sebelum _deploy_ index ke production.

## Soft Delete

Pakai `deleted_at timestamp NULL DEFAULT NULL`.
Query default pakai `WHERE deleted_at IS NULL`.

## Connection Pool

- Go: `SetMaxOpenConns(25)`, `SetMaxIdleConns(10)`.
- NestJS/Prisma: Default pool cukup, adjust berdasarkan load.
