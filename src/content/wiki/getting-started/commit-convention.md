---
title: "Commit Convention"
description: "Standarisasi commit message dengan Conventional Commits dan commitlint."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["git", "commit", "conventional-commits", "commitlint"]
isPinned: true
growthStage: "evergreen"
---

Semua commit **WAJIB** mengikuti [Conventional Commits](https://www.conventionalcommits.org/).
Validasi otomatis dengan **commitlint** via lefthook.

## Format

```
<type>(<scope>): <description>

<body>

<footer>
```

## Commit Types

| Type       | Kegunaan                                 |
| ---------- | ---------------------------------------- |
| `feat`     | Fitur baru                               |
| `fix`      | Perbaikan bug                            |
| `chore`    | Tugas maintenance, dependencies, tooling |
| `docs`     | Perubahan dokumentasi                    |
| `refactor` | Refaktor kode (bukan fitur/fix)          |
| `style`    | Perubahan format (spasi, prettier, dll)  |
| `test`     | Nambah atau perbaiki test                |
| `perf`     | Optimasi performa                        |
| `ci`       | Perubahan CI config                      |
| `build`    | Build system atau dependency             |
| `revert`   | Revert commit sebelumnya                 |

## Scope

Scope opsional, gunakan untuk konteks tambahan:

```
feat(auth): add OAuth2 Google login
fix(api): handle empty request body
chore(deps): upgrade eslint to v9
```

## Contoh Baik

```
feat: add student registration endpoint

Implement POST /api/v1/students dengan validasi NIM.
Integrasi dengan queue service untuk sending email.
```

```
fix(parser): handle null input in date formatter

Closes #42
```

```
chore: setup lefthook and commitlint

- Add lefthook for pre-commit hooks
- Add commitlint with conventional config
- Configure parallel lint + format jobs
```

## Aturan

1. **Pakai bahasa Inggris** untuk konsistensi
2. **Imperative mood**: "add" bukan "added"/"adds"
3. **Deskripsi singkat** (maks 72 karakter)
4. **Jangan titik** di akhir description
5. Body untuk menjelaskan **kenapa**, bukan **apa**

## Config

`commitlint.config.mjs`:

```js
export default {
  extends: ["@commitlint/config-conventional"],
};
```
