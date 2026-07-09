---
title: "Performance Optimization"
description: "Optimasi performa untuk frontend dan backend."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["performance", "optimization", "frontend", "backend"]
isPinned: false
growthStage: "evergreen"
---

## Frontend

### Bundle Size

- Gunakan **dynamic imports** untuk code splitting:

```tsx
const Chart = dynamic(() => import("./Chart"), {
  loading: () => <Skeleton />,
});
```

- Analisis bundle dengan `pnpm exec vite-bundle-visualizer`.
- Tree-shake dependencies yang tidak perlu.

### Images

- Gunakan `<Image>` component (Next.js / Astro):

```astro
<Image src={photo} width={800} height={600} format="webp" quality={80} />
```

- Serve WebP/AVIF dengan fallback.
- Lazy load images di bawah fold.

### Fonts

- Gunakan `font-display: swap` untuk prevent FOIT.
- Subset font hanya karakter yang digunakan.
- Preload critical fonts:

```html
<link
  rel="preload"
  href="/fonts/inter.woff2"
  as="font"
  type="font/woff2"
  crossorigin
/>
```

### CSS

- Purge unused Tailwind classes di production.
- Minimize CSS dengan build tool.
- Hindari layout thrashing (batch DOM reads/writes).

## Backend

### Database

- Index kolom yang sering di-`WHERE`, `JOIN`, `ORDER BY`.
- Use `EXPLAIN ANALYZE` sebelum deploy query baru.
- Batch insert/update untuk bulk operations:

```go
// Go: batch insert
db.CreateInBatches(users, 100)
```

### API Response

- Pagination untuk list endpoints.
- Field selection ( GraphQL-style atau query param `?fields=id,name` ).
- Compression: enable gzip/brotli di web server.

### Connection Pooling

| Stack   | Default    | Production |
| ------- | ---------- | ---------- |
| Go      | 25 open    | 50 open    |
| Node.js | 10         | 20         |
| PHP     | persistent | per-worker |

## Metrics

Track Core Web Vitals:

| Metric  | Target  | Tool                   |
| ------- | ------- | ---------------------- |
| LCP     | < 2.5s  | Lighthouse, Web Vitals |
| FID/INP | < 200ms | Chrome UX Report       |
| CLS     | < 0.1   | Lighthouse             |
| TTFB    | < 600ms | Server logs            |

## Monitoring

Gunakan tools untuk monitoring di production:

- **Frontend**: Vercel Analytics, Google Analytics 4
- **Backend**: Prometheus + Grafana (Go), APM tools
- **Database**: pg_stat_statements, slow query log
