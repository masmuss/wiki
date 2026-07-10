---
title: "Bun & Hono Development Standards"
description: "Standar development untuk Bun + Hono stack."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["bun", "hono", "typescript", "backend"]
isPinned: false
growthStage: "evergreen"
---

## Project Layout

```
project/
├── src/
│   ├── index.ts          # Entry point
│   ├── routes/           # Route handlers
│   ├── middleware/       # Hono middleware
│   ├── db/               # Drizzle / raw SQL
│   ├── types/            # TypeScript types
│   └── utils/            # Helpers
├── tests/
├── drizzle/              # Migrations
├── docker/
├── package.json
├── bun.lockb
└── tsconfig.json
```

## Hono Router

```ts
import { Hono } from "hono";

const app = new Hono();

app.get("/users", async (c) => {
  const users = await db.select().from(usersTable);
  return c.json({ data: users });
});

app.post("/users", async (c) => {
  const body = await c.req.json();
  const user = await db.insert(usersTable).values(body).returning();
  return c.json({ data: user }, 201);
});

export default app;
```

## Validation

Gunakan **Zod** untuk validasi:

```ts
import { z } from "zod";

const createUserSchema = z.object({
  name: z.string().min(2),
  email: z.string().email(),
});

app.post("/users", async (c) => {
  const body = await c.req.json();
  const parsed = createUserSchema.safeParse(body);

  if (!parsed.success) {
    return c.json({ error: parsed.error.flatten() }, 400);
  }

  // ...
});
```

## Database

Gunakan **Drizzle ORM** dengan PostgreSQL:

```ts
import { drizzle } from "drizzle-orm/node-postgres";

const db = drizzle(process.env.DATABASE_URL!);
```

## Testing

Gunakan **Bun test runner** (built-in):

```ts
import { describe, it, expect } from "bun:test";

describe("user routes", () => {
  it("creates a user", async () => {
    const res = await app.request("/users", {
      method: "POST",
      body: JSON.stringify({ name: "Khoirul", email: "test@example.com" }),
    });

    expect(res.status).toBe(201);
  });
});
```

## Docker Multi-Stage

Gunakan Bun multi-stage build untuk image kecil:

```dockerfile
FROM oven/bun:1 AS base
WORKDIR /app
COPY package.json bun.lockb ./
RUN bun install --frozen-lockfile
COPY . .
RUN bun build ./src/index.ts --outdir ./out

FROM oven/bun:1-slim
WORKDIR /app
COPY --from=base /app/out .
EXPOSE 3000
CMD ["bun", "run", "index.js"]
```

## Hot Reload

```bash
bun --hot run src/index.ts
```
