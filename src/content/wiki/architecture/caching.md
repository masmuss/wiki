---
title: "Caching Strategy"
description: "Strategi caching untuk performa aplikasi."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["caching", "redis", "performance"]
isPinned: false
growthStage: "evergreen"
---

### 1. Browser Cache (CDN)

Static assets (CSS, JS, images) cache di CDN:

```
Cache-Control: public, max-age=31536000, immutable
```

### 2. Application Cache (Redis)

Cache data yang sering diakses dan jarang berubah:

| Data Type       | TTL      | Invalidation        |
| --------------- | -------- | ------------------- |
| User profile    | 1 jam    | On update           |
| Product catalog | 15 menit | On CRUD             |
| Session         | 24 jam   | On logout           |
| API response    | 5 menit  | Cache key + version |

### 3. Database Cache (Query Cache)

Gunakan built-in query cache jika tersedia (PostgreSQL tidak punya built-in query cache, gunakan Redis layer).

### 4. In-Memory Cache

Gunakan untuk data hyper-local per request:

```go
// Go: simple in-memory cache
var cache = make(map[string]interface{})
var mu sync.RWMutex

func GetCached(key string) (interface{}, bool) {
    mu.RLock()
    defer mu.RUnlock()
    v, ok := cache[key]
    return v, ok
}
```

## Redis Patterns

### Cache-Aside (Lazy Loading)

```
1. Cek cache
2. Kalau miss → query DB → store ke cache
3. Return data
```

```go
func GetUser(ctx context.Context, id string) (*User, error) {
    // 1. Cek cache
    key := fmt.Sprintf("user:%s", id)
    cached, err := redis.Get(ctx, key).Result()
    if err == nil {
        var user User
        json.Unmarshal([]byte(cached), &user)
        return &user, nil
    }

    // 2. Query DB
    user, err := db.FindUser(ctx, id)
    if err != nil {
        return nil, err
    }

    // 3. Store ke cache
    data, _ := json.Marshal(user)
    redis.Set(ctx, key, data, time.Hour)

    return user, nil
}
```

### Write-Through

```
1. Write ke DB
2. Write ke cache (update/invalidate)
3. Return
```

```go
func UpdateUser(ctx context.Context, id string, data UpdateUserInput) error {
    // 1. Update DB
    if err := db.UpdateUser(ctx, id, data); err != nil {
        return err
    }

    // 2. Invalidate cache
    key := fmt.Sprintf("user:%s", id)
    redis.Del(ctx, key)

    return nil
}
```

## Cache Key Naming

```
<resource>:<id>:<version>
<resource>:list:<filter_hash>
```

Contoh:

```
user:123:v1
products:list:category=electronics_page=1
```

## Stale-While-Revalidate

Untuk frontend, gunakan pattern SWR dengan TanStack Query:

```ts
const { data } = useQuery({
  queryKey: ["users"],
  queryFn: fetchUsers,
  staleTime: 5 * 60 * 1000, // 5 menit
  cacheTime: 10 * 60 * 1000, // 10 menit
});
```

## When NOT to Cache

- Data real-time (stock prices, chat messages)
- Data user-private yang sensitif
- Data yang sering berubah (< 1 menit)
- Large files (video, bulk data)
