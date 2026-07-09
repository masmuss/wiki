---
title: "API Design Standards"
description: "Standarisasi desain REST API untuk semua backend service."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["standards", "api", "rest"]
isPinned: false
growthStage: "evergreen"
---

## URL Structure

Gunakan **plural nouns** untuk resource:

```
GET    /api/v1/users          # List users
POST   /api/v1/users          # Create user
GET    /api/v1/users/:id      # Get user by ID
PUT    /api/v1/users/:id      # Update user
DELETE /api/v1/users/:id      # Delete user
GET    /api/v1/users/:id/posts # Nested resource
```

## Versioning

Gunakan prefix `/v1/`, `/v2/` di URL.
Hanya increment kalau ada breaking changes.

## Response Format

### Success

Semua response sukses mengikuti envelope yang konsisten:

```json
{
  "data": { ... } | [ ... ],
  "meta": { ... }    // optional, hanya untuk collection/list
}
```

#### Single Object (`data` adalah objek)

Untuk endpoint yang return satu resource:

```json
{
  "data": {
    "id": "usr_2v6d8x1",
    "name": "Ahmad Musafir",
    "email": "khoirul@example.com",
    "role": "admin",
    "created_at": "2026-07-01T08:00:00Z"
  }
}
```

Contoh endpoint:

```
GET    /api/v1/users/:id      → { data: User }
POST   /api/v1/users          → { data: User }              // 201
PUT    /api/v1/users/:id      → { data: User }
DELETE /api/v1/users/:id      → { data: null }              // 204, body optional
```

#### Collection / Array (`data` adalah array)

Untuk endpoint yang return list:

```json
{
  "data": [
    {
      "id": "usr_2v6d8x1",
      "name": "Ahmad Musafir",
      "email": "khoirul@example.com"
    },
    {
      "id": "usr_9k3m5p2",
      "name": "John Doe",
      "email": "john@example.com"
    }
  ],
  "meta": {
    "page": 1,
    "per_page": 20,
    "total": 150,
    "total_pages": 8
  }
}
```

Contoh endpoint:

```
GET /api/v1/users             → { data: User[], meta: PaginationMeta }
GET /api/v1/users/:id/posts   → { data: Post[], meta: PaginationMeta }
GET /api/v1/search            → { data: Result[], meta: PaginationMeta }
```

#### Aturan `data`

| Kondisi                        | Bentuk `data`             | `meta`    |
| ------------------------------ | ------------------------- | --------- |
| Single resource (GET by ID)    | **Object**                | Tidak ada |
| Create / Update / Delete       | **Object**                | Tidak ada |
| List / Search / Filter         | **Array**                 | Wajib ada |
| Empty list                     | **Array kosong** `[]`     | Wajib ada |
| Endpoint non-resource (health) | **Object** atau primitive | Tidak ada |

> [!WARNING]
> **Jangan** kirim object langsung tanpa `data` wrapper. **Jangan** kirim array langsung di root tanpa envelope. Konsistensi envelope memudahkan client handling global.

### Error

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Validation failed",
    "details": [
      {
        "field": "email",
        "message": "Email is required"
      }
    ]
  }
}
```

## HTTP Status Codes

| Code | Description       | When                                |
| ---- | ----------------- | ----------------------------------- |
| 200  | OK                | GET, PUT sukses                     |
| 201  | Created           | POST sukses                         |
| 204  | No Content        | DELETE sukses                       |
| 400  | Bad Request       | Validasi gagal                      |
| 401  | Unauthorized      | Belum login                         |
| 403  | Forbidden         | Tidak punya akses                   |
| 404  | Not Found         | Resource tidak ditemukan            |
| 409  | Conflict          | Duplicate data                      |
| 422  | Unprocessable     | Validasi business logic             |
| 429  | Too Many Requests | Rate limit                          |
| 500  | Internal Error    | Server error (jangan expose detail) |

## Pagination

Gunakan **cursor-based** untuk list besar, **page-based** untuk list kecil:

```json
{
  "data": [...],
  "meta": {
    "page": 1,
    "per_page": 20,
    "total": 150,
    "total_pages": 8
  }
}
```

Query params: `?page=1&per_page=20` atau `?cursor=eyJpZCI6MTB9&limit=20`.

## Authentication

Gunakan **Bearer token** di header:

```
Authorization: Bearer <token>
```

## Rate Limiting

Return header:

```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 99
X-RateLimit-Reset: 1620000000
```
