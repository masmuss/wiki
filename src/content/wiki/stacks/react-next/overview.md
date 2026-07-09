---
title: "React & Next.js Development Standards"
description: "Standar development untuk React dan Next.js."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["react", "nextjs", "frontend"]
isPinned: false
growthStage: "evergreen"
---

## Project Layout

```
project/
├── src/
│   ├── app/              # Next.js App Router
│   ├── components/       # Reusable components
│   ├── lib/              # Utils, hooks
│   ├── hooks/            # Custom hooks
│   └── types/            # TypeScript types
├── public/
├── tests/
├── next.config.js
└── package.json
```

## App Router

Gunakan **App Router** (bukan Pages Router) untuk semua project baru.

```tsx
// app/users/page.tsx
export default async function UsersPage() {
  const users = await getUsers();

  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

## Data Fetching

Gunakan **TanStack Query** untuk client-side fetching:

```tsx
"use client";

import { useQuery } from "@tanstack/react-query";

export function UserList() {
  const { data, isLoading } = useQuery({
    queryKey: ["users"],
    queryFn: fetchUsers,
  });

  if (isLoading) return <Spinner />;
  return (
    <ul>
      {data?.map((u) => (
        <li key={u.id}>{u.name}</li>
      ))}
    </ul>
  );
}
```

## Component Patterns

- **Functional components** saja.
- **Custom hooks** untuk logic reusable.
- **Server Components** default, **Client Components** hanya untuk interaktif.

```tsx
// Server Component (default)
export default function UserCard({ user }: Props) {
  return <div>{user.name}</div>;
}

// Client Component (interactive)
("use client");

export function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount((c) => c + 1)}>{count}</button>;
}
```

## State Management

- **Local state**: `useState`, `useReducer`
- **Server state**: **TanStack Query**
- **Global state**: **Zustand** (lightweight) atau Context API untuk simple case
- **Jangan pakai Redux** kecuali benar-benar perlu.

## Styling

Gunakan **Tailwind CSS** atau **shadcn/ui**.

```tsx
// Tailwind
<button className="rounded-lg bg-blue-600 px-4 py-2 text-white hover:bg-blue-700">
  Submit
</button>
```

## Testing

```bash
# Unit test
pnpm vitest

# E2E
pnpm playwright test
```

## Routing

App Router structure:

```
app/
├── (auth)/             # Route group
│   ├── login/
│   └── register/
├── dashboard/
│   ├── layout.tsx
│   └── page.tsx
├── api/
│   └── users/
│       └── route.ts    # API route
└── layout.tsx
```
