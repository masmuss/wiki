---
title: "Troubleshooting Guide"
description: "Common issues dan solusi di semua stack."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["troubleshooting", "debugging"]
isPinned: false
growthStage: "budding"
---

## Node.js / pnpm

### `ENOENT` / `MODULE_NOT_FOUND`

```bash
rm -rf node_modules pnpm-lock.yaml
pnpm install
```

### Port already in use

```bash
# Cari proses yang pakai port
lsof -i :3000
# Kill proses
kill -9 <PID>
```

### TypeScript errors setelah upgrade package

```bash
# Clear cache
pnpm store prune
rm -rf node_modules
pnpm install
```

## Go

### `go mod tidy` hangs

```bash
# Set proxy
export GOPROXY=https://proxy.golang.org,direct
export GOSUMDB=sum.golang.org
go mod tidy
```

### Race condition

```bash
# Detect dengan race detector
go test ./... -race
```

### Build failed: undefined variable

Cek apakah file ada di package yang benar. Go tidak compile file yang `_test.go` atau file dengan build tags yang tidak match.

## Docker

### Container won't start

```bash
# Cek logs
docker logs <container_id>

# Cek env
docker inspect <container_id> | grep -A 20 Env
```

### Permission denied (volume)

```bash
# Fix permission
docker exec -it <container_id> chown -R 1000:1000 /app
```

## Database

### Migration failed

```bash
# Cek status migration
# Go / Atlas
atlas migrate status

# Laravel
php artisan migrate:status

# Drizzle
npx drizzle-kit check
```

### Connection refused

- Cek apakah DB container running: `docker ps`
- Cek port: `lsof -i :5432`
- Cek connection string di `.env`

## Git

### Merge conflict

```bash
# Abort merge
git merge --abort

# Lihat conflicted files
git diff --name-only --diff-filter=U

# Resolve, lalu
git add .
git commit -m "merge: resolve conflicts"
```

### Commit stuck di pre-commit hook

```bash
# Cek lefthook log
lefthook run pre-commit -v

# Skip hook (emergency only)
git commit --no-verify -m "..."
```

## Editor (Zed)

### LSP not working

1. Cek `zed: open log` untuk error LSP.
2. Restart LSP: `editor: restart language server`
3. Cek apakah binary LSP ada di PATH.

### Format on save not working

- Cek settings.json: `format_on_save` harus `"on"`.
- Cek apakah formatter binary ada (Biome, Prettier).
- Cek `languages.TypeScript.formatter` config.
