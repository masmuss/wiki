---
title: "Go Development Standards"
description: "Standar development untuk Go: layout, DI, testing, dan patterns."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["go", "golang", "backend"]
isPinned: false
growthStage: "evergreen"
---

## Project Layout

```
project/
├── cmd/                  # Entry point per binary
│   └── server/
│       └── main.go
├── internal/             # Private code
│   ├── domain/           # Entity, use case interface
│   ├── application/      # Service, DTO
│   ├── infrastructure/   # Handler, repo, middleware
│   └── config/           # Config loader
├── pkg/                  # Shared, reusable code
├── migrations/           # DB migrations (atlas, goose)
├── tests/                # Integration tests
├── Makefile
├── go.mod
└── go.sum
```

## Dependency Injection

Gunakan **uber-go/fx** untuk wiring otomatis.

```go
func main() {
    fx.New(
        config.Module,
        database.Module,
        modules.UserModule,
        fx.Invoke(func(*server.Server) {}),
    ).Run()
}
```

## API Handler (Chi)

```go
func (h *Handler) CreateUser(w http.ResponseWriter, r *http.Request) {
    var req CreateUserRequest
    if err := json.NewDecoder(r.Body).Decode(&req); err != nil {
        respond.Error(w, http.StatusBadRequest, "invalid body")
        return
    }

    user, err := h.service.Create(r.Context(), req)
    if err != nil {
        respond.Error(w, http.StatusInternalServerError, err.Error())
        return
    }

    respond.OK(w, user)
}
```

## Error Handling

```go
// Return early
if err != nil {
    return fmt.Errorf("failed to create user: %w", err)
}

// Wrap context
return fmt.Errorf("user %s: %w", userID, ErrNotFound)
```

## Testing

- Unit test: `go test ./internal/...`
- Integration test: `go test ./tests/... -tags integration`
- Coverage target: **> 70%**

```go
func TestCreateUser(t *testing.T) {
    repo := newMockUserRepo()
    svc := NewUserService(repo)

    user, err := svc.Create(ctx, CreateUserRequest{Name: "Khoirul"})
    assert.NoError(t, err)
    assert.Equal(t, "Khoirul", user.Name)
}
```

## Tools

| Tool          | Command                   | Fungsi             |
| ------------- | ------------------------- | ------------------ |
| Air           | `air`                     | Hot reload         |
| Delve         | `dlv debug`               | Debugger           |
| golangci-lint | `golangci-lint run ./...` | Comprehensive lint |
| goimports     | `goimports -w .`          | Auto import sort   |

## Orm / Query Builder

- **Ent** (Facebook): Type-safe, generated code
- **GORM**: Prototyping dan rapid dev
- **SQLx**: Raw SQL dengan scanning
- **Raw SQL**: Untuk query kompleks / performance critical

## Config

Gunakan struct tag + environment variables:

```go
type Config struct {
    Port     int    `env:"PORT,default=8080"`
    DBHost   string `env:"DB_HOST,required"`
    LogLevel string `env:"LOG_LEVEL,default=info"`
}
```
