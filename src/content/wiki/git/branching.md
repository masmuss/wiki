---
title: "Git Branching Strategy"
description: "Standard branching model dan naming convention."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["git", "branching", "workflow"]
isPinned: true
growthStage: "evergreen"
---

## Branch Hierarchy

```
main (production)
  └── dev (integration)
       ├── feat/xxx
       ├── fix/xxx
       └── chore/xxx
```

| Branch       | Base   | Purpose                       |
| ------------ | ------ | ----------------------------- |
| `main`       | -      | Production-ready, deployed    |
| `dev`        | `main` | Integration, feature complete |
| `feat/*`     | `dev`  | New features                  |
| `fix/*`      | `dev`  | Bug fixes                     |
| `chore/*`    | `dev`  | Maintenance, deps, tooling    |
| `refactor/*` | `dev`  | Code refactoring              |

## Workflow

1. Branch dari `dev`:
   ```bash
   git checkout dev && git pull
   git checkout -b feat/your-feature
   ```
2. Kerja, commit, push
3. Bikin PR ke `dev`
4. PR di-review, squash & merge
5. `dev` → `main` untuk release (merge commit, bukan squash)

## Branch Naming

Format: `<type>/<kebab-case-description>`

```
feat/student-registration
fix/null-pointer-on-parse
chore/upgrade-deps
docs/api-readme
```

Detail lihat [[naming-conventions]].
