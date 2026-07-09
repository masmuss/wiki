---
title: "Design System"
description: "Standar UI/UX, komponen, dan design tokens."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["design", "ui", "tailwind"]
isPinned: false
growthStage: "evergreen"
---

## Filosofi

- **Utility-first** dengan Tailwind CSS.
- **Shadcn/ui** sebagai base component library.
- **Consistent spacing** dengan scale 4px (0.25rem).
- **Dark mode first** atau minimal support dark mode.

## Color Tokens

Gunakan Tailwind config atau CSS variables:

```css
:root {
  --background: 0 0% 100%;
  --foreground: 222.2 84% 4.9%;
  --primary: 222.2 47.4% 11.2%;
  --primary-foreground: 210 40% 98%;
  --accent: 217.2 32.6% 17.5%;
  --accent-foreground: 210 40% 98%;
  --muted: 210 40% 96.1%;
  --muted-foreground: 215.4 16.3% 46.9%;
  --destructive: 0 84.2% 60.2%;
  --border: 214.3 31.8% 91.4%;
  --ring: 222.2 84% 4.9%;
  --radius: 0.5rem;
}
```

## Typography

| Element | Font           | Size     | Weight |
| ------- | -------------- | -------- | ------ |
| H1      | System / Sans  | 2.25rem  | 700    |
| H2      | System / Sans  | 1.875rem | 600    |
| H3      | System / Sans  | 1.5rem   | 600    |
| Body    | System / Sans  | 1rem     | 400    |
| Small   | System / Sans  | 0.875rem | 400    |
| Mono    | JetBrains Mono | 0.875rem | 400    |

## Spacing Scale

Gunakan Tailwind spacing scale:

| Token | Value         |
| ----- | ------------- |
| 1     | 0.25rem (4px) |
| 2     | 0.5rem (8px)  |
| 4     | 1rem (16px)   |
| 6     | 1.5rem (24px) |
| 8     | 2rem (32px)   |
| 12    | 3rem (48px)   |
| 16    | 4rem (64px)   |

## Component Patterns

### Button

```tsx
// Variants: primary, secondary, ghost, destructive
// Sizes: sm, default, lg, icon
<Button variant="primary" size="default">
  Submit
</Button>
```

### Form Input

```tsx
<div className="space-y-2">
  <Label htmlFor="email">Email</Label>
  <Input id="email" type="email" placeholder="you@example.com" />
  <p className="text-sm text-muted-foreground">We'll never share your email.</p>
</div>
```

## Responsive Breakpoints

| Breakpoint | Width   | Tailwind Prefix |
| ---------- | ------- | --------------- |
| Mobile     | < 640px | (default)       |
| Tablet     | 640px   | `sm:`           |
| Desktop    | 768px   | `md:`           |
| Wide       | 1024px  | `lg:`           |
| Ultra-wide | 1280px  | `xl:`           |

## Dark Mode

Gunakan `class` strategy dengan Tailwind:

```ts
// tailwind.config.ts
darkMode: "class";
```

Toggle via class di `<html>`:

```html
<html class="dark"></html>
```

## Accessibility

- Semua interactive element harus bisa diakses keyboard.
- Color contrast ratio minimal **4.5:1** untuk text.
- Gunakan semantic HTML (`<button>` bukan `<div onclick>`).
- Add `aria-label` untuk icon-only buttons.
