---
title: "Deployment Guide"
description: "Strategi deployment untuk berbagai platform dan stack."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["deployment", "devops", "docker"]
isPinned: false
growthStage: "evergreen"
---

## Platform Pilihan

| Project Type | Platform Utama      | Alternatif       |
| ------------ | ------------------- | ---------------- |
| Static/Astro | Vercel / Cloudflare | GitHub Pages     |
| Next.js      | Vercel              | Netlify          |
| SvelteKit    | Vercel / Netlify    | Cloudflare Pages |
| Go API       | VPS + Docker        | Fly.io, Railway  |
| Laravel      | VPS + Docker        | Laravel Cloud    |
| Bun/Hono     | VPS + Docker        | Fly.io           |
| NestJS       | VPS + Docker        | Railway          |

## Environment Strategy

```
development → staging → production
```

| Env     | Branch    | Purpose                    |
| ------- | --------- | -------------------------- |
| Dev     | `dev`     | Development, testing       |
| Staging | `staging` | Pre-prod validation        |
| Prod    | `main`    | Production, tagged release |

## Docker Deployment

### Multi-Stage Build (Go)

```dockerfile
FROM golang:1.22-alpine AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o server ./cmd/server

FROM gcr.io/distroless/static-debian11
WORKDIR /app
COPY --from=builder /app/server .
EXPOSE 8080
CMD ["./server"]
```

### Bun Multi-Stage

```dockerfile
FROM oven/bun:1 AS builder
WORKDIR /app
COPY package.json bun.lockb ./
RUN bun install --frozen-lockfile
COPY . .
RUN bun build ./src/index.ts --outdir ./out --target node

FROM oven/bun:1-slim
WORKDIR /app
COPY --from=builder /app/out .
EXPOSE 3000
CMD ["bun", "run", "index.js"]
```

### Docker Compose (Production)

```yaml
services:
  app:
    build: .
    ports:
      - "8080:8080"
    environment:
      - DATABASE_URL=${DATABASE_URL}
      - REDIS_URL=${REDIS_URL}
    depends_on:
      - db
      - redis

  db:
    image: postgres:16-alpine
    volumes:
      - pgdata:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine

volumes:
  pgdata:
```

## Environment Variables

1. **Never commit `.env`** files.
2. **Gunakan `.env.example`** dengan dummy values.
3. **Production secrets**: gunakan Docker secrets, cloud provider secret manager, atau env injection.

```bash
# .env.example
DATABASE_URL=postgresql://user:pass@localhost:5432/dbname
REDIS_URL=redis://localhost:6379
JWT_SECRET=change-me-in-production
PORT=8080
```

## Checklist Pre-Deploy

- [ ] Semua test passing
- [ ] Lint & type check bersih
- [ ] Migration dijalankan (jika ada DB change)
- [ ] Environment variables sudah di-set
- [ ] Health check endpoint tersedia
- [ ] Logging sudah configured
- [ ] Rollback plan siap

## Health Check

Setiap service WAJIB punya health check endpoint:

```go
// Go
app.Get("/health", func(c *fiber.Ctx) error {
    return c.JSON(fiber.Map{"status": "ok"})
})
```

```ts
// Hono
app.get("/health", (c) => c.json({ status: "ok", uptime: process.uptime() }));
```

## Rollback

Gunakan Docker image tags untuk rollback cepat:

```bash
# Deploy specific version
docker pull registry/app:v1.2.3
docker-compose up -d

# Rollback
docker pull registry/app:v1.2.2
docker-compose up -d
```
