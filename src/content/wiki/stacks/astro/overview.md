---
title: "Astro Development Standards"
description: "Standar development untuk Astro projects."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["astro", "ssg", "frontend"]
isPinned: false
growthStage: "evergreen"
---

## Astro Documentation

Untuk dokumentasi lengkap, kunjungi [Astro Documentation](https://docs.astro.build/).

## Project Layout

```
project/
├── src/
│   ├── assets/             # Static assets (images, fonts, styles etc.)
│   │   ├── styles/         # Global CSS
│   │   │   ├── global.css  # Global CSS
│   │   │   ├── tokens.css  # Tokens CSS
│   │   ├── images/         # Images
│   │   └── fonts/          # Fonts
│   ├── features/           # Feature components
│   │   ├── home/           # Home page components
│   │   │   ├── components/ # Home page components
│   │   │   ├── views/      # Home page components
│   │   │   └── types.ts    # Home page types
│   ├── components/         # UI components (.astro / .tsx / .svelte)
│   │   ├── shell/          # Shell components
│   │   ├── layout/         # Layout components
│   │   └── shared/         # Shared components
│   ├── layouts/            # Page layouts
│   ├── pages/              # Routing
│   ├── content/            # Content collections
│   ├── lib/                # Utils
│   └── content.config.ts   # Content collections configuration
├── public/                 # Static assets
├── astro.config.mjs
└── package.json
```

## Island Architecture

Gunakan framework components hanya untuk interaktif area.

```astro
---
// Mostly static page
import BaseLayout from "../layouts/Base.astro";
---

<BaseLayout>
  <h1>Welcome</h1>
  <!-- Static content here -->

  <!-- Interactive island -->
  <Counter client:load />
  <Search client:idle />
</BaseLayout>
```

## Content Collections

Gunakan content collections untuk blog, wiki, docs:

```ts
// src/content.config.ts
import { defineCollection, z } from "astro:content";

const blog = defineCollection({
  schema: z.object({
    title: z.string(),
    pubDate: z.date(),
    tags: z.array(z.string()),
  }),
});

export const collections = { blog };
```

```astro
---
import { getCollection } from "astro:content";
const posts = await getCollection("blog");
---

<ul>
  {
    posts.map((p) => (
      <li>
        <a href={`/blog/${p.id}`}>{p.data.title}</a>
      </li>
    ))
  }
</ul>
```

## Routing

- File-based routing di `src/pages/`.
- Dynamic routes: `[id].astro`, `[...slug].astro`.
- API routes: `src/pages/api/*.ts` dengan `export GET`, `export POST`, dll.

## Styling

- **Tailwind CSS** default untuk styling.
- **Scoped styles** di `.astro` files:

```astro
<style>
  h1 {
    color: blue;
  }
</style>
```

## Deployment

- **Static**: `output: 'static'` (default) — deploy ke Vercel, Cloudflare Pages, GitHub Pages.
- **SSR**: `output: 'server'` dengan adapter (Vercel, Node).

## Dev Command

```bash
# Dev server
astro dev

# Background mode
astro dev --background
astro dev stop
astro dev status
astro dev logs
```
