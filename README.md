# 🌲 Veka Wiki

**Personal Development Standards & Knowledge Base — powered by the [Veka](https://github.com/masmuss/veka) template.**

A curated wiki documenting software development standards, architecture decisions, stack-specific guidelines, and tooling configurations used across personal projects. Built with Astro for fast static rendering and zero-config routing.

![Astro](https://img.shields.io/badge/Astro-3.0+-FF7E33?style=flat-square&logo=astro&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-v4-38B2AC?style=flat-square&logo=tailwind-css&logoColor=white)
![Pagefind](https://img.shields.io/badge/Search-Pagefind-blue?style=flat-square)

---

## ⚡ Why This Wiki?

- **Centralized Standards:** Single source of truth for code conventions, architecture patterns, and stack-specific best practices.
- **Zero-Config Routing:** Powered by [Veka](https://github.com/masmuss/veka) — create nested folders (`src/content/wiki/stacks/golang/`) and navigation auto-generates.
- **Ultra-Fast Static Search:** Pagefind indexes content during build — no third-party search service required.
- **Growth Tracking:** Notes tagged with `growthStage` (`seedling` → `budding` → `evergreen`) to track maturity.

---

## 🚀 Quick Start

```bash
# 1. Clone
pnpm dlx degit khoirul/veka my-wiki

# 2. Install
cd my-wiki && pnpm install

# 3. Dev server
pnpm run dev

# 4. Build (required for Pagefind search)
pnpm run build && pnpm run preview
```

---

## 📚 Wiki Structure

```
src/content/wiki/
├── getting-started/          # Local env setup, tooling config
├── standards/                # Naming, project structure, API design
├── git/                      # Branching, workflow, collaboration
├── architecture/             # Clean architecture, DI, database, caching
├── stacks/                   # Per-tech guidelines
│   ├── golang/
│   ├── laravel/
│   ├── svelte/
│   ├── react-next/
│   ├── astro/
│   ├── bun-hono/
│   ├── flutter/
│   └── nestjs/
├── guides/                   # Testing, deployment, CI/CD, design system
└── notes/                    # Performance, security, troubleshooting
```

---

## 🏗️ Project Structure

```
├── src/
│   ├── content/
│   │   ├── config.ts         # Zod schema validation
│   │   └── wiki/             # Markdown content
│   ├── layouts/
│   │   ├── BaseLayout.astro  # SEO & Meta shell
│   │   └── WikiLayout.astro  # 3-Column grid (Nav, Content, TOC)
│   └── pages/
│       ├── index.astro       # Dashboard / Home
│       └── wiki/
│           └── [...slug].astro # Dynamic routing engine
├── lefthook.yml              # Git hooks (lint, format, commitlint)
├── commitlint.config.mjs     # Conventional commits
└── .prettierrc               # Code formatting
```

---

## 🌍 Deployment

Static site, deployable to Vercel, Netlify, or Cloudflare Pages.

Build command: `pnpm run build`

---

## 🙏 Acknowledgements

This project is built on top of **[Veka](https://github.com/masmuss/veka)** — a minimalist digital garden & wiki starter for Astro by [Masmuss](https://github.com/masmuss).

---

## License

MIT
