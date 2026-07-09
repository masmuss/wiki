---
title: "Git Hooks dengan Lefthook"
description: "Konfigurasi git hooks otomatis untuk menjaga kualitas kode."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["git", "hooks", "lefthook", "automation"]
isPinned: false
growthStage: "evergreen"
---

Kita menggunakan **Lefthook** untuk mengelola git hooks.
Lefthook dipilih karena cepat (Go-based), support parallel execution, dan konfigurasi sederhana.

## Instalasi

```bash
pnpm add -D lefthook
pnpm lefthook install
```

Hook `prepare` sudah otomatis jalan di `pnpm install` via `"prepare": "lefthook install"` di `package.json`.

## Konfigurasi

File: `lefthook.yml`

```yaml
pre-commit:
  parallel: true
  jobs:
    - name: lint
      run: pnpm lint {staged_files}
      glob: "*.{js,ts,mjs,cjs,astro}"
    - name: format
      run: pnpm format --check {staged_files}
      glob: "*.{js,ts,mjs,cjs,astro,json,yaml,yml}"

commit-msg:
  jobs:
    - name: commitlint
      run: pnpm commitlint --edit {1}
```

### Penjelasan

| Hook         | Job          | Fungsi                            |
| ------------ | ------------ | --------------------------------- |
| `pre-commit` | `lint`       | ESLint cuma di file yang di-stage |
| `pre-commit` | `format`     | Cek formatting dengan Prettier    |
| `commit-msg` | `commitlint` | Validasi commit message           |

Semua job di `pre-commit` jalan **paralel** biar cepat.

## Skipping Hooks

Gunakan `--no-verify` hanya untuk darurat:

```bash
git commit --no-verify -m "wip: fix later"
```

Untuk skip hook tertentu:

```bash
LEFTHOOK_EXCLUDE=lint git commit -m "feat: skip lint"
```
