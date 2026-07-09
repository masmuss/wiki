---
title: "Laravel Development Standards"
description: "Standar development untuk Laravel dan Filament."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["laravel", "php", "filament"]
isPinned: false
growthStage: "evergreen"
---

## Project Layout

Laravel 11+ slim structure:

```
project/
├── app/
│   ├── Domain/          # Entity, repository interface
│   ├── Application/     # Service, DTO
│   └── Infrastructure/  # Eloquent repo, Controller, Filament
├── bootstrap/
├── config/
├── database/
│   ├── factories/
│   ├── migrations/
│   └── seeders/
├── resources/
│   └── views/
├── routes/
│   ├── web.php
│   └── api.php
├── storage/
├── tests/
│   ├── Feature/
│   └── Unit/
├── composer.json
└── .env.example
```

## Repository Pattern

```php
interface UserRepositoryInterface
{
    public function find(int $id): ?User;
    public function create(array $data): User;
}

class EloquentUserRepository implements UserRepositoryInterface
{
    public function __construct(private User $model) {}

    public function find(int $id): ?User
    {
        return $this->model->find($id);
    }
}
```

Bind di `AppServiceProvider`:

```php
$this->app->bind(
    UserRepositoryInterface::class,
    EloquentUserRepository::class
);
```

## Filament

Gunakan Filament untuk admin panel dan resource management.

```php
class UserResource extends Resource
{
    protected static ?string $model = User::class;
    protected static ?string $navigationIcon = 'heroicon-o-users';
}
```

## Testing

- Feature test untuk HTTP endpoint.
- Unit test untuk domain logic.
- Use `RefreshDatabase` trait.

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

## Tools

| Tool    | Command                        | Fungsi          |
| ------- | ------------------------------ | --------------- |
| Pint    | `./vendor/bin/pint`            | Formatting      |
| PHPStan | `./vendor/bin/phpstan analyse` | Static analysis |
| Sail    | `sail up`                      | Docker dev      |

## Config

- Cache config di production: `php artisan config:cache`
- Route cache: `php artisan route:cache`
- Never commit `.env`.
