---
title: "Deployment Guide"
description: "Strategi deployment untuk berbagai platform dan stack."
createdAt: 2026-07-09
updatedAt: 2026-07-10
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

## Docker

Setup Docker production-ready untuk VPS deployment:

[[docker]] - Multi-stage Dockerfile, PostgreSQL, Redis, Docker Compose, Health Check, Log Aggregation, Resource Limits, Non-Root, GHCR, Security Scanning.

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
- [ ] Docker image sudah di-build dan di-scan
- [ ] Health check endpoint tersedia
- [ ] Logging sudah configured (stdout/json)
- [ ] Resource limits sudah di-set
- [ ] Rollback plan siap
- [ ] Docker registry image pushed

## Rollback

Gunakan Docker image tags untuk rollback cepat:

```bash
# Deploy specific version
docker pull ghcr.io/<username>/app:v1.2.3
docker compose up -d --no-deps app

# Rollback
docker pull ghcr.io/<username>/app:v1.2.2
docker compose up -d --no-deps app
```
