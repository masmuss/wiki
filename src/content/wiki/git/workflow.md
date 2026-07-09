---
title: "Git Workflow & Collaboration"
description: "Daily workflow, code review, dan pull request process."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["git", "workflow", "pr", "code-review"]
isPinned: false
growthStage: "evergreen"
---

## Daily Workflow

```bash
# 1. Sync dev
git checkout dev && git pull

# 2. Buat branch fitur
git checkout -b feat/your-feature

# 3. Kerja dan commit
git add .
git commit -m "feat(scope): meaningful message"

# 4. Push dan buat PR
git push -u origin feat/your-feature
```

## Commit Practice

- **Commit sering**: setiap logic selesai
- **Squash sebelum PR**: rapikan commit history
- **Gunakan [[commit-convention]]**: semua commit harus valid

### Rebasing

```bash
# Sync dengan dev sebelum PR
git rebase dev

# Interactive rebase untuk squash
git rebase -i HEAD~5
```

## Pull Request

Setiap PR harus:

1. **Judul jelas**: pakai conventional commit format
2. **Deskripsi**: apa yang diubah dan kenapa
3. **Testing evidence**: screenshot atau log
4. **Link issue** jika ada: `Closes #42`

### PR Checklist

- [ ] Kode mengikuti [[naming-conventions]]
- [ ] Tidak ada warning ESLint/TypeScript
- [ ] Test sudah jalan (`pnpm test` / `go test ./...`)
- [ ] Tidak ada kode yang di-comment
- [ ] Branch sudah di-rebase dengan `dev`

## Code Review

### Untuk Reviewer

- Fokus ke **logic**, bukan formatting (itu tugas linter)
- Cari **edge cases** yang terlewat
- Pastikan **error handling** proper
- Cek apakah ada **side effects** tak terduga

### Untuk Author

- Minta review ke **minimal 1 orang** (atau tunggu 24 jam)
- Jelaskan konteks di deskripsi PR
- Respons cepat ke comments

## Merge Strategy

| Branch Merge | Strategy         | Alasan                                  |
| ------------ | ---------------- | --------------------------------------- |
| `feat`→`dev` | **Squash merge** | Satu commit rapi per fitur              |
| `dev`→`main` | **Merge commit** | Pertahankan history untuk release trace |
