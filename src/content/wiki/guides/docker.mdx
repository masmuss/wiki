---
title: "Docker Deployment"
description: "Production Docker setup untuk semua stack."
createdAt: 2026-07-10
updatedAt: 2026-07-10
tags: ["docker", "devops", "deployment"]
isPinned: false
growthStage: "evergreen"
---

Dokumentasi ini menjelaskan setup Docker production-ready untuk semua stack yang digunakan. Semua service berjalan di VPS dengan Docker Compose.

## .dockerignore

Sebelum build, pastikan file yang tidak diperlukan tidak ikut masuk ke dalam image. Ini membuat image lebih kecil dan lebih aman.

```gitignore
node_modules
.git
.env
.env.*
dist
*.log
README.md
```

## Multi-Stage Dockerfiles

Multi-stage build memisahkan build stage dan runtime stage. Builder stage berisi toolchain lengkap untuk compile. Runtime stage hanya berisi artifacts yang dibutuhkan untuk execute. Hasilnya: image lebih kecil, attack surface lebih kecil.

### NestJS

```dockerfile
FROM node:22-alpine AS builder
WORKDIR /app
COPY package.json pnpm-lock.yaml ./
RUN corepack prepare pnpm@latest --activate && pnpm install --frozen-lockfile
COPY . .
RUN pnpm build

FROM node:22-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production

COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package.json ./package.json

RUN addgroup -g 1001 -S nodejs && adduser -S nestjs -u 1001 -G nodejs
USER nestjs

EXPOSE 3000
CMD ["node", "dist/main.js"]
```

Runtime stage tidak menginstall dependencies baru. Cukup copy `node_modules` dari builder stage karena production butuh modules yang sama. User `nestjs` dibuat tanpa login shell untuk security.

### Laravel

Laravel butuh dua container: PHP-FPM untuk memproses PHP, dan Nginx untuk melayani HTTP. PHP-FPM container menjalankan composer install, Nginx container melayani static files dan me-proxy request ke PHP-FPM.

```dockerfile
FROM php:8.3-fpm-alpine AS builder
WORKDIR /var/www/html
RUN docker-php-ext-install pdo pdo_pgsql

COPY . .

RUN composer install --no-dev --optimize-autoloader

FROM nginx:1.26-alpine
WORKDIR /var/www/html

COPY --from=builder /var/www/html /var/www/html
COPY docker/nginx.conf /etc/nginx/conf.d/default.conf

# www-data is already created in alpine images, just switch to it
USER www-data

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

**`docker/nginx.conf`:**

```nginx
server {
    listen 80;
    root /var/www/html/public;
    index index.php index.html;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        fastcgi_pass php-fpm:9000;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}
```

Nginx mendengarkan di port 80. Semua request `.php` di-proxy ke `127.0.0.1:9000` tempat PHP-FPM berjalan. Karena Nginx melayani static files secara langsung, PHP-FPM hanya dipanggil untuk dynamic requests.

### Astro Static

Astro build menghasilkan static HTML files. Melayani dengan Nginx cukup karena tidak ada server-side logic.

```dockerfile
FROM node:22-alpine AS builder
WORKDIR /app
COPY package.json pnpm-lock.yaml ./
RUN corepack prepare pnpm@latest --activate && pnpm install --frozen-lockfile
COPY . .
RUN pnpm build

FROM nginx:1.26-alpine
WORKDIR /usr/share/nginx/html
COPY --from=builder /app/dist ./
COPY docker/nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

**`docker/nginx.conf`:**

```nginx
server {
    listen 80;
    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    gzip on;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml;
}
```

`try_files` fallback ke `/index.html` untuk mendukung client-side routing Astro. Gzip compression diaktifkan untuk text assets agar transfer lebih cepat.

### Bun/Hono

```dockerfile
FROM oven/bun:1 AS builder
WORKDIR /app
COPY package.json bun.lockb ./
RUN bun install --frozen-lockfile
COPY . .
RUN bun build ./src/index.ts --outdir ./out --target bun

FROM oven/bun:1-slim
WORKDIR /app
ENV NODE_ENV=production

COPY --from=builder /app/out .
COPY --from=builder /app/package.json .

RUN addgroup -g 1001 -S bunjs && adduser -S bunjs -u 1001 -G bunjs
USER bunjs

EXPOSE 3000
CMD ["bun", "run", "index.js"]
```

Bun runtime lebih ringan dari Node. Gunakan `oven/bun:1-slim` untuk production image. Bundle output dari builder stage di-copy ke runtime stage tanpa toolchain build.

---

## PostgreSQL

### Init Script

PostgreSQL menjalankan scripts di `/docker-entrypoint-initdb.d/` sekali saja saat first startup. Gunanya untuk membuat extensions, setup initial schemas, atau membuat users tambahan.

```yaml
db:
  image: postgres:16-alpine
  environment:
    POSTGRES_USER: appuser
    POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
    POSTGRES_DB: appdb
  volumes:
    - pgdata:/var/lib/postgresql/data
    - ./docker/init.sql:/docker-entrypoint-initdb.d/init.sql:ro
  command: postgres -c max_connections=100 -c shared_buffers=128MB
```

`max_connections` dan `shared_buffers` di-set via command flags, bukan `postgresql.conf`. Ini lebih portable karena tidak perlu mount config file terpisah. Sesuaikan nilai dengan resource VPS.

**`docker/init.sql`:**

```sql
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
CREATE EXTENSION IF NOT EXISTS "pg_trgm";

-- Performance indexes
CREATE INDEX IF NOT EXISTS idx_users_email ON users(email);
CREATE INDEX IF NOT EXISTS idx_users_created_at ON users(created_at);
```

`CREATE EXTENSION` harus dijalankan oleh superuser atau user dengan privilege yang sesuai. Init script berjalan sebagai `postgres` superuser. `CONCURRENTLY` membuat index creation tidak memblokir writes.

### Backup Volume

Backup automation menggunakan cron-like loop. Setiap 24 jam, `pg_dump` membuat compressed binary backup. Backup yang lebih tua dari 7 hari di-delete otomatis.

```yaml
volumes:
  pgdata:
  pgbackups:

services:
  db:
    image: postgres:16-alpine
    volumes:
      - pgdata:/var/lib/postgresql/data
      - pgbackups:/backups

  backup:
    image: postgres:16-alpine
    volumes:
      - pgbackups:/backups
    environment:
      - PGHOST=db
      - PGUSER=${POSTGRES_USER}
      - PGPASSWORD=${POSTGRES_PASSWORD}
      - PGDATABASE=${POSTGRES_DB}
    entrypoint: ["sh", "-c"]
    command: |
      "while true; do
        pg_dump -Fc > /backups/backup_$(date +%Y%m%d_%H%M%S).psql;
        find /backups -name 'backup_*.psql' -mtime +7 -delete;
        sleep 86400;
      done"
```

Untuk restore: `pg_restore -d appdb /backups/backup_YYYYMMDD_HHMMSS.psql`

---

## Redis

### Production Config

Redis di production butuh memory limit dan eviction policy. `allkeys-lru` menghapus key yang paling lama tidak diakses ketika memory limit tercapai. Ini mencegah Redis dari OOM kills.

```yaml
redis:
  image: redis:7-alpine
  command: redis-server --maxmemory 512mb --maxmemory-policy allkeys-lru --appendonly yes
  volumes:
    - redisdata:/data
```

`appendonly yes` mengaktifkan AOF (Append Only File) untuk durability. Data di-append ke file, tidak ditulis ulang setiap operation. `appendfsync everysec` adalah kompromi antara performance dan durability.

### Redis Auth + Persistence

Jika Redis terekspose ke network (bukan `localhost` only), wajib menggunakan password.

```yaml
redis:
  image: redis:7-alpine
  command: >
    redis-server
    --requirepass ${REDIS_PASSWORD}
    --maxmemory 512mb
    --maxmemory-policy allkeys-lru
    --appendonly yes
    --appendfsync everysec
  volumes:
    - redisdata:/data
```

`REDIS_URL` di application code harus menyertakan password: `redis://:password@redis:6379`

---

## Docker Compose Production

Konfigurasi production lengkap dengan semua service, health checks, resource limits, dan logging config.

```yaml
services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    restart: unless-stopped
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=${DATABASE_URL}
      - REDIS_URL=${REDIS_URL}
    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_started
    healthcheck:
      test: ["CMD", "wget", "-q", "--spider", "http://localhost:3000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s
    deploy:
      resources:
        limits:
          cpus: "1.0"
          memory: 512M
        reservations:
          cpus: "0.25"
          memory: 128M
    logging:
      driver: json-file
      options:
        max-size: "10m"
        max-file: "3"

  db:
    image: postgres:16-alpine
    restart: unless-stopped
    environment:
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
      POSTGRES_DB: ${POSTGRES_DB}
    volumes:
      - pgdata:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER} -d ${POSTGRES_DB}"]
      interval: 10s
      timeout: 5s
      retries: 5
    deploy:
      resources:
        limits:
          cpus: "1.5"
          memory: 1G
        reservations:
          cpus: "0.5"
          memory: 256M
    logging:
      driver: json-file
      options:
        max-size: "50m"
        max-file: "5"

  redis:
    image: redis:7-alpine
    restart: unless-stopped
    command: redis-server --maxmemory 512mb --maxmemory-policy allkeys-lru --appendonly yes
    volumes:
      - redisdata:/data
    deploy:
      resources:
        limits:
          cpus: "0.5"
          memory: 512M
        reservations:
          cpus: "0.1"
          memory: 64M
    logging:
      driver: json-file
      options:
        max-size: "10m"
        max-file: "3"

volumes:
  pgdata:
  redisdata:
```

**Penjelasan key points:**

- `restart: unless-stopped` - container restart otomatis setelah reboot atau crash
- `depends_on` dengan `condition: service_healthy` - app tidak start sebelum db benar-benar ready
- `deploy.resources` - hard limits untuk CPU dan memory, reservation adalah guaranteed minimum (Catatan: gunakan flag `--compatibility` saat run `docker compose` jika limit tidak direalisasikan di luar Swarm mode).
- `logging` dengan rotation - mencegah disk penuh akibat log file yang terlalu besar

---

## Health Check

### Docker HEALTHCHECK Instruction

Health check di Dockerfile digunakan Docker untuk memantau container health. Jika check gagal sebanyak `retries` kali berturut-turut, container dianggap unhealthy dan akan di-restart.

```dockerfile
HEALTHCHECK --interval=30s --timeout=10s --start-period=40s --retries=3 \
  CMD wget -q --spider http://localhost:3000/health || exit 1
```

`start-period` memberi waktu 40 detik untuk app start sebelum check pertama. Ini mencegah false positives saat app masih booting.

### App Health Endpoint Examples

Health endpoint mengembalikan JSON dengan status dan metadata. Menyertakan `uptime` atau `version` berguna untuk debugging.

```go
// Go / Fiber
app.Get("/health", func(c *fiber.Ctx) error {
    return c.JSON(fiber.Map{
        "status":  "ok",
        "version": "1.0.0",
        "uptime":  time.Since(start).String(),
    })
})
```

```ts
// Hono / Bun
app.get("/health", (c) =>
  c.json({
    status: "ok",
    version: process.env.npm_package_version,
    uptime: process.uptime(),
  }),
);
```

```ts
// NestJS
import { Controller, Get } from "@nestjs/common";

@Controller()
export class HealthController {
  @Get("health")
  check() {
    return { status: "ok", timestamp: new Date().toISOString() };
  }
}
```

Health endpoint HARUS lightweight. Jangan query database atau cache di dalam health check. Cukup return in-memory status.

---

## Log Aggregation

### Principles

Container logs harus keluar ke `stdout` / `stderr`. Docker menangkap output ini dan menulisnya ke log file. Jangan menulis logs ke files di dalam container karena:

1. Files di container hilang saat container di-recreate
2. Tidak bisa di-rotate dengan Docker's logging driver
3. Menyulitkan log aggregation

JSON log format lebih mudah di-parse oleh log aggregation tools (ELK, Loki, CloudWatch).

### Go JSON Logging

`slog` adalah structured logger built-in Go 1.21+. Output ke `os.Stdout` agar Docker menangkap.

```go
import "log/slog"

func main() {
    slog.SetDefault(slog.New(slog.NewJSONHandler(os.Stdout, nil)))
    slog.Info("server started", "port", 8080)
}
```

### Node.js JSON Logging

```ts
import pino from "pino";

const logger = pino({ level: "info" });
logger.info({ event: "server_start", port: 3000 }, "server started");
```

### Laravel JSON Logging

Laravel default monolog dapat dikonfigurasi untuk output JSON ke stdout.

```php
// config/logging.php
'stack' => [
    'driver' => 'monolog',
    'handler' => Monolog\Handler\StreamHandler::class,
    'formatter' => Monolog\Formatter\JsonFormatter::class,
    'with' => [
        'stream' => 'php://stdout',
    ],
],
```

### Docker Logging Driver

```yaml
logging:
  driver: json-file
  options:
    max-size: "10m"
    max-file: "3"
```

`json-file` driver me-rotate log files otomatis. Maksimal 3 files, masing-masing maksimal 10MB. Total maksimal 30MB logs per container.

Untuk production dengan central log aggregation, gunakan `fluentd` driver:

```yaml
logging:
  driver: "fluentd"
  options:
    fluentd-address: localhost:24224
    tag: "app.{{.Name}}"
```

---

## Resource Limits

Setiap service HARUS punya explicit resource limits. Tanpa limits, satu container bisa mengonsumsi semua memory VPS dan mengakibatkan service lain ter-kill.

| Service        | CPU Limit | Memory Limit | Notes                         |
| -------------- | --------- | ------------ | ----------------------------- |
| app (Node/Bun) | 1.0       | 512M         | JS heap + event loop          |
| app (Go)       | 1.0       | 256M         | Go manages memory efficiently |
| app (PHP)      | 1.0       | 256M         | PHP-FPM pools                 |
| PostgreSQL     | 1.5       | 1G           | Buffer pool + connections     |
| Redis          | 0.5       | 512M         | Key store + AOF buffer        |
| Nginx          | 0.25      | 128M         | Static serving                |

Reservation adalah guaranteed minimum. Limit adalah maximum. Jika VPS penuh, Docker menghormati reservation. App yang burst melampaui reservation tapi masih dalam limit mungkin mengalami degradation.

### Ulimits

File descriptors dan process limits mencegah fork bombs dan file handle exhaustion.

```yaml
services:
  app:
    ulimits:
      nofile:
        soft: 65536
        hard: 65536
      nproc:
        soft: 4096
        hard: 4096
```

---

## Non-Root User

Container berjalan sebagai root secara default. Ini berbahaya jika attacker berhasil compromise container — mereka punya root access ke host. Solusinya: selalu buat non-root user dan switch ke user tersebut di akhir Dockerfile.

```dockerfile
# Create group and user
RUN addgroup -g 1001 -S appuser && adduser -S appuser -u 1001 -G appuser

# Switch to non-root
USER appuser
```

Alpine's `adduser` dengan flag `-S` membuat system user tanpa login shell. User ID 1001 adalah standar untuk Node.js images.

**Port numbers:** Linux tidak mengizinkan non-root user bind ke ports < 1024. Solusinya: expose port > 1024 (misal 3000) dan map ke 80/443 di docker-compose atau reverse proxy.

---

## Docker Registry (GHCR)

GitHub Container Registry (GHCR) gratis untuk public packages. Untuk private packages, menggunakan GitHub Actions minutes. Registry URL format: `ghcr.io/username/repo:image-tag`

### Build & Push

```bash
# Login
echo $GHCR_TOKEN | docker login ghcr.io -u khoirul --password-stdin

# Build with tag
docker build -t ghcr.io/khoirul/appname:v1.0.0 .

# Push
docker push ghcr.io/khoirul/appname:v1.0.0
```

Untuk regenerate token: GitHub → Settings → Developer Settings → Personal Access Tokens → Fine-grained tokens → Generate new token dengan scope `write:packages`.

### GitHub Actions Registry Login

```yaml
- name: Login to GHCR
  uses: docker/login-action@v3
  with:
    registry: ghcr.io
    username: ${{ github.actor }}
    password: ${{ secrets.GITHUB_TOKEN }}
```

`GITHUB_TOKEN` adalah built-in secret, tidak perlu dibuat manual. Token ini valid selama workflow run berjalan.

### Pull & Run

```bash
docker pull ghcr.io/khoirul/appname:v1.0.0
docker run -d -p 3000:3000 --name app ghcr.io/khoirul/appname:v1.0.0
```

---

## Security Scanning

Image scanning menemukan vulnerabilities di OS packages dan application dependencies sebelum deploy ke production.

### Docker Scout (Recommended)

Docker Scout adalah scanner yang terintegrasi dengan Docker Desktop dan CLI. Tidak perlu instalasi tambahan untuk local scanning.

```bash
# Enable
docker scout enable

# Quick view
docker scout cves ghcr.io/khoirul/appname:v1.0.0

# Full report
docker scout sbom ghcr.io/khoirul/appname:v1.0.0
docker scout compare ghcr.io/khoirul/appname:v1.0.0 --to ghcr.io/khoirul/appname:v1.1.0
```

`compare` command berguna untuk melihat apa yang berubah antar versi image.

### Trivy

Trivy adalah scanner open-source dari Aqua Security. Bisa scan images, Dockerfile, dan IaC configs.

```bash
# Scan image
trivy image ghcr.io/khoirul/appname:v1.0.0

# Scan Dockerfile
trivy config dockerfile:Dockerfile

# CI gate
trivy image --exit-code 1 --severity HIGH,CRITICAL ghcr.io/khoirul/appname:v1.0.0
```

Exit code 1 ketika vulnerabilities ditemukan membuat CI pipeline fail secara otomatis.

### GitHub Actions Integration

```yaml
- name: Run Trivy
  uses: aquasecurity/trivy-action@master
  with:
    image-ref: ghcr.io/khoirul/appname:v1.0.0
    format: sarif
    output: trivy-results.sarif
    severity: HIGH,CRITICAL

- name: Upload to GitHub Security
  uses: github/codeql-action/upload-sarif@v3
  with:
    sarif_file: trivy-results.sarif
```

SARIF format memungkinkan GitHub's Security tab menampilkan vulnerabilities dengan code references.

### CI Gate Rules

Tidak semua vulnerabilities sama. Gunakan risk-based approach:

- **Critical**: Block deploy segera. Contoh: RCE, sensitive data exposure, authentication bypass.
- **High**: Block jika exploitation mudah atau dampaknya luas. Contoh: remote code execution, privilege escalation.
- **Medium**: Track, fix dalam sprint. Contoh: information disclosure, DoS yang butuh kondisi khusus.
- **Low**: Acknowledge, fix saat ada kesempatan. Contoh: minor information leaks, hardening opportunities.
