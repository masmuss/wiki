---
title: "Analytics & Monitoring"
description: "Setup analytics dan monitoring untuk aplikasi."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["analytics", "monitoring", "observability"]
isPinned: false
growthStage: "seedling"
---

## Frontend Analytics

### Google Analytics 4 (GA4)

Setup untuk Astro / Next.js / SvelteKit:

```ts
// lib/analytics.ts
export function trackEvent(name: string, params?: Record<string, any>) {
    if (typeof window !== 'undefined' && window.gtag) {
        window.gtag('event', name, params);
    }
}

// Usage
<button onclick={() => trackEvent('download_cv', { format: 'pdf' })}>
    Download CV
</button>
```

### Privacy-First Alternatif

- **Plausible**: Lightweight, no cookie, open source.
- **Umami**: Self-hosted analytics.
- **PostHog**: Product analytics + feature flags.

## Backend Monitoring

### Structured Logging

Go dengan slog:

```go
logger := slog.New(slog.NewJSONHandler(os.Stdout, nil))
logger.Info("user_created", "user_id", user.ID, "email", user.Email)
```

Output:

```json
{
  "time": "2026-07-09T10:00:00Z",
  "level": "INFO",
  "msg": "user_created",
  "user_id": "123",
  "email": "test@example.com"
}
```

### Metrics

Prometheus + Grafana untuk Go services:

```go
import "github.com/prometheus/client_golang/prometheus"

var requestDuration = prometheus.NewHistogramVec(
    prometheus.HistogramOpts{
        Name: "http_request_duration_seconds",
        Help: "HTTP request duration",
    },
    []string{"method", "path", "status"},
)
```

### Health Checks

Setiap service harus expose:

```
GET /health → {"status": "ok"}
GET /ready → {"status": "ok"}  // DB connected, dependencies ready
```

## Error Tracking

- **Sentry**: Universal error tracking (JS, Go, PHP, Flutter).
- Setup di production only:

```ts
// Astro / Next.js
import * as Sentry from "@sentry/browser";

Sentry.init({
  dsn: import.meta.env.PUBLIC_SENTRY_DSN,
  environment: import.meta.env.MODE,
  tracesSampleRate: 0.1,
});
```

## Alerts

Setup alerting untuk:

- Error rate > 1%
- Response time > 500ms (p95)
- Database connection pool > 80%
- Disk usage > 85%
- Memory usage > 90%

Gunakan PagerDuty, Opsgenie, atau Slack webhook.
