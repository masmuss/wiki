---
title: "Naming Conventions"
description: "Standarisasi penamaan untuk file, branch, variabel, dan komponen."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["standards", "naming"]
isPinned: true
growthStage: "evergreen"
---

## File & Directory Names

| Language / Context    | Convention                                 | Examples                     |
| --------------------- | ------------------------------------------ | ---------------------------- |
| TypeScript/JavaScript | kebab-case                                 | `user-profile.tsx`           |
| Astro components      | kebab-case (or PascalCase for re-exported) | `card.astro`, `Button.astro` |
| Go                    | snake_case                                 | `user_repository.go`         |
| PHP / Laravel         | snake_case                                 | `user_profile.php`           |
| Svelte components     | PascalCase                                 | `UserCard.svelte`            |
| React components      | PascalCase                                 | `UserCard.tsx`               |
| CSS / SCSS            | kebab-case                                 | `btn-primary.css`            |

## Branch Naming

```
<type>/<description>
```

| Type        | Contoh                                |
| ----------- | ------------------------------------- |
| `feat/`     | `feat/student-registration`           |
| `fix/`      | `fix/null-pointer-on-parser`          |
| `chore/`    | `chore/upgrade-eslint`                |
| `refactor/` | `refactor/extract-validation-service` |
| `docs/`     | `docs/api-endpoints`                  |

Gunakan **kebab-case** dan deskripsi singkat.

## Variable & Function Naming

| Language   | Variables              | Functions                                  | Constants   | Interfaces/Types |
| ---------- | ---------------------- | ------------------------------------------ | ----------- | ---------------- |
| TypeScript | camelCase              | camelCase                                  | UPPER_SNAKE | PascalCase       |
| Go         | camelCase              | PascalCase (exported), camelCase (private) | UPPER_SNAKE | PascalCase       |
| PHP        | camelCase / snake_case | camelCase / snake_case                     | UPPER_SNAKE | PascalCase       |
| Svelte     | camelCase              | camelCase                                  | UPPER_SNAKE | PascalCase       |

### Boolean naming

Gunakan prefix positif:

- ✅ `isLoading`, `hasError`, `canSubmit`, `showModal`
- ❌ `loading`, `error`, `notVisible`

### Go-specific

- Acronyms: `HTTPHandler`, `UserID`, bukan `HttpHandler`, `UserId`
- Getter methods: jangan pakai `Get` prefix (`user.Name()` bukan `user.GetName()`)
- Interface: akhiri dengan `-er` untuk behavior (`Storer`, `Fetcher`, `Logger`)

## Commit Message

Lihat [[commit-convention]] untuk detail lengkap.
