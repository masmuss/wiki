---
title: "Wiki Home"
description: "Central knowledge base for software development standards, architecture, and specifications."
createdAt: 2026-07-06
updatedAt: 2026-07-09
tags: ["hub", "index"]
isPinned: true
growthStage: "evergreen"
---

# 🌲 Development Wiki

Wiki ini berisi standarisasi, spesifikasi, dan best practices untuk semua stack yang digunakan.

---

## 🚀 Getting Started

Setup local environment, toolchain, dan konfigurasi dasar.

| Topic                                        | Description                            |
| -------------------------------------------- | -------------------------------------- |
| [[overview\|Local Environment Setup]]        | Prasyarat runtime dan global tools     |
| [[git-hooks\|Git Hooks]]                     | Lefthook, pre-commit, commit-msg hooks |
| [[commit-convention\|Commit Convention]]     | Conventional Commits + commitlint      |
| [[linting-formatting\|Linting & Formatting]] | ESLint, Prettier, Biome                |
| [[editor-config\|Editor Configuration]]      | Zed, Neovim, Starship, Ghostty         |

---

## 📋 Standards

Standarisasi penamaan, struktur, dan desain yang berlaku untuk semua project.

| Topic                                      | Description                                       |
| ------------------------------------------ | ------------------------------------------------- |
| [[naming-conventions\|Naming Conventions]] | File, branch, variable, function naming           |
| [[project-structure\|Project Structure]]   | Root files, monorepo, README standards            |
| [[api-design\|API Design]]                 | REST URL structure, response format, status codes |

---

## 🔀 Git

Best practices untuk branching, commit, dan kolaborasi.

| Topic                                      | Description                               |
| ------------------------------------------ | ----------------------------------------- |
| [[branching\|Branching Strategy]]          | Branch hierarchy, feature branch workflow |
| [[workflow\|Git Workflow & Collaboration]] | Daily workflow, PR, code review           |

---

## 🏗️ Architecture

Prinsip arsitektur untuk semua backend service.

| Topic                                          | Description                            |
| ---------------------------------------------- | -------------------------------------- |
| [[clean-architecture\|Clean Architecture]]     | Layer separation, dependency rule      |
| [[dependency-injection\|Dependency Injection]] | DI patterns: Go (fx), Laravel, NestJS  |
| [[database-design\|Database Design]]           | PostgreSQL schema, migration, indexing |
| [[caching\|Caching Strategy]]                  | Redis, cache-aside, in-memory, CDN     |

---

## 🛠️ Tech Stacks

Standar development per teknologi.

| Stack                                           | Description                                   |
| ----------------------------------------------- | --------------------------------------------- |
| [[stacks/golang/overview\|Go]]                  | Layout, fx, Chi, testing, Ent/GORM            |
| [[stacks/laravel/overview\|Laravel]]            | Repository pattern, Filament, testing         |
| [[stacks/svelte/overview\|Svelte / SvelteKit]]  | Svelte 5 runes, state, TanStack Query         |
| [[stacks/react-next/overview\|React / Next.js]] | App Router, TanStack Query, Tailwind          |
| [[stacks/astro/overview\|Astro]]                | Islands, content collections, routing         |
| [[stacks/bun-hono/overview\|Bun + Hono]]        | Hono router, Drizzle, Zod, multi-stage Docker |
| [[stacks/flutter/overview\|Flutter]]            | Clean arch, BLoC, GoRouter                    |
| [[stacks/nestjs/overview\|NestJS]]              | Module pattern, Prisma, DTO validation        |

---

## 📖 Guides

Panduan praktis lintas stack.

| Topic                                  | Description                                |
| -------------------------------------- | ------------------------------------------ |
| [[onboarding\|New Project Onboarding]] | Checklist setup project baru               |
| [[testing\|Testing Standards]]         | Unit, integration, E2E untuk semua stack   |
| [[deployment\|Deployment Guide]]       | Docker, VPS, Vercel, platform pilihan      |
| [[ci-cd\|CI/CD Pipeline]]              | GitHub Actions workflows                   |
| [[design-system\|Design System]]       | Tailwind, shadcn/ui, tokens, accessibility |

---

## 📝 Notes

Referensi teknis dan troubleshooting.

| Topic                                      | Description                             |
| ------------------------------------------ | --------------------------------------- |
| [[performance\|Performance Optimization]]  | Frontend & backend optimization         |
| [[security\|Security Standards]]           | Secrets, validation, XSS, SQLi, headers |
| [[troubleshooting\|Troubleshooting Guide]] | Common issues dan solusi                |
| [[analytics\|Analytics & Monitoring]]      | GA4, Sentry, Prometheus, logging        |

---

_Last updated: 2026-07-09_
