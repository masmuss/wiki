---
title: "Svelte Development Standards"
description: "Standar development untuk SvelteKit dan Svelte 5."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["svelte", "sveltekit", "frontend"]
isPinned: false
growthStage: "evergreen"
---

## Project Layout

```
project/
├── src/
│   ├── lib/
│   │   ├── components/   # Reusable UI
│   │   ├── stores/       # State management
│   │   └── utils/        # Helpers
│   ├── routes/           # SvelteKit routing
│   ├── app.html
│   └── app.css
├── static/
├── tests/
├── package.json
└── vite.config.ts
```

## Svelte 5 Runes

Gunakan runes untuk reactivity:

```svelte
<script>
    let count = $state(0);
    let doubled = $derived(count * 2);

    $effect(() => {
        console.log(`Count is now ${count}`);
    });
</script>

<button onclick={() => count++}>
    Clicks: {count} (doubled: {doubled})
</button>
```

## Component Patterns

- **Props**: Jangan pakai spread props kecuali perlu.
- **Events**: Gunakan callback props, bukan `createEventDispatcher`.
- **Snippets**: Gunakan snippets (Svelte 5) untuk komposisi:

```svelte
{#snippet icon()}
    <Icon name="check" />
{/snippet}

<Button>{@render icon()}</Button>
```

## State Management

Gunakan **Svelte stores** untuk global state, **$state** untuk local.

```ts
// stores/auth.ts
import { writable } from 'svelte/store';

export const user = writable<User | null>(null);

// Component
<script>
    import { user } from '$lib/stores/auth';
</script>

<p>Hello {$user?.name}</p>
```

## Testing

Gunakan **Vitest** untuk unit test, **Playwright** untuk E2E.

```ts
// Component test
import { render, screen } from "@testing-library/svelte";
import Counter from "./Counter.svelte";

test("increment button", async () => {
  render(Counter);
  const button = screen.getByRole("button");
  await fireEvent.click(button);
  expect(button).toHaveTextContent("1");
});
```

## Tailwind

SvelteKit + Tailwind CSS setup:

```css
/* app.css */
@import "tailwindcss";
```

Utility-first, jangan bikin custom CSS kecuali perlu.

## API Loading

Gunakan **TanStack Query** (atau SvelteKit `+page.server.ts` + `+page.ts`):

```ts
// +page.ts
export const load = async ({ fetch }) => {
  const res = await fetch("/api/users");
  return { users: await res.json() };
};
```
