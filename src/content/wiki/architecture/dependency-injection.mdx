---
title: "Dependency Injection"
description: "Pattern DI untuk Go, Laravel, dan NestJS."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["architecture", "di", "dependency-injection"]
isPinned: false
growthStage: "evergreen"
---

Dependency Injection (DI) memudahkan testing, maintenance, dan decoupling antar layer.

## Go

Gunakan **uber-go/fx** atau **samber/do**.

### fx Pattern

```go
// Module definition
var UserModule = fx.Module("user",
    fx.Provide(NewUserRepository),
    fx.Provide(NewUserService),
    fx.Provide(NewUserHandler),
)

// Provide (constructor injection)
func NewUserService(repo UserRepository) *UserService {
    return &UserService{repo: repo}
}
```

### fx Module

Group module ke dalam `internal/modules/` atau `internal/domain/`.

## Laravel

Gunakan service container bawaan.

```php
// Bind interface ke implementation
$this->app->bind(
    UserRepositoryInterface::class,
    EloquentUserRepository::class
);

// Inject via constructor
class UserController extends Controller
{
    public function __construct(
        private UserRepositoryInterface $users
    ) {}
}
```

## NestJS

Gunakan module + decorator `@Injectable()`.

```typescript
@Module({
  providers: [UserService],
  controllers: [UserController],
})
export class UserModule {}

@Injectable()
export class UserService {
  constructor(private repo: UserRepository) {}
}
```

## Aturan

1. **Interface ke implementation**: Kode depend ke interface, bukan class konkret.
2. **Constructor injection**: Jangan pakai property injection / setter injection.
3. **No global state**: Gak boleh pakai singleton / global variable kecuali via DI container.
