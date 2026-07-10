---
title: "Testing Standards"
description: "Standar testing untuk semua stack: unit, integration, dan E2E."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["testing", "qa", "standards"]
isPinned: false
growthStage: "evergreen"
---

## Prinsip

1. **Test pyramid**: Banyak unit test, sedikit E2E.
2. **Coverage target**: Minimal **70%** untuk backend, **60%** untuk frontend.
3. **Test harus cepat**: Unit test < 100ms per test.
4. **Jangan test implementation**: Test behavior, bukan internal structure.

## Unit Testing

### Go

Gunakan testing bawaan + testify:

```go
func TestCreateUser(t *testing.T) {
    repo := newMockRepo()
    svc := NewUserService(repo)

    user, err := svc.Create(ctx, CreateUserRequest{Name: "Khoirul"})
    require.NoError(t, err)
    assert.Equal(t, "Khoirul", user.Name)
}
```

Run:

```bash
go test ./internal/... -cover
go test ./... -tags integration  # integration tests
```

### TypeScript / JavaScript

Gunakan **Vitest** untuk unit test:

```ts
import { describe, it, expect } from "vitest";
import { calculateTotal } from "./utils";

describe("calculateTotal", () => {
  it("sums items correctly", () => {
    expect(calculateTotal([10, 20, 30])).toBe(60);
  });

  it("returns 0 for empty array", () => {
    expect(calculateTotal([])).toBe(0);
  });
});
```

### PHP / Laravel

```php
class UserTest extends TestCase
{
    use RefreshDatabase;

    public function test_user_can_be_created()
    {
        $response = $this->postJson('/api/users', [
            'name' => 'Khoirul',
            'email' => 'test@example.com',
        ]);

        $response->assertCreated()
            ->assertJsonPath('data.name', 'Khoirul');
    }
}
```

## Integration Testing

Test API endpoint dengan database real (test container atau SQLite in-memory):

### Go

```go
func TestCreateUserIntegration(t *testing.T) {
    db := setupTestDB(t)
    handler := NewUserHandler(db)

    req := httptest.NewRequest("POST", "/users", body)
    rec := httptest.NewRecorder()
    handler.Create(rec, req)

    assert.Equal(t, http.StatusCreated, rec.Code)
}
```

### Hono / Bun

```ts
describe("POST /users", () => {
  it("creates user", async () => {
    const res = await app.request("/users", {
      method: "POST",
      body: JSON.stringify({ name: "Khoirul", email: "test@example.com" }),
    });
    expect(res.status).toBe(201);
  });
});
```

## E2E Testing

Gunakan **Playwright** untuk web apps:

```ts
import { test, expect } from "@playwright/test";

test("user can login", async ({ page }) => {
  await page.goto("/login");
  await page.fill('[name="email"]', "test@example.com");
  await page.fill('[name="password"]', "password");
  await page.click('button[type="submit"]');
  await expect(page).toHaveURL("/dashboard");
});
```

Run:

```bash
pnpm exec playwright test
```

## Test File Naming

| Type        | Pattern                   |
| ----------- | ------------------------- |
| Unit        | `*.test.ts` / `*_test.go` |
| Integration | `*.integration.test.ts`   |
| E2E         | `*.spec.ts` (Playwright)  |

## Mocking

- Go: interface + manual mock atau mockery
- TS: vitest `vi.fn()`
- PHP: Mockery atau Laravel mocking

## CI Integration

Test selalu jalan di CI sebelum merge. Lihat [[ci-cd]] untuk detail pipeline.
