---
title: "Security Standards"
description: "Security checklist dan best practices untuk semua stack."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["security", "standards"]
isPinned: true
growthStage: "evergreen"
---

## Secrets Management

- **Never commit secrets** ke repository.
- Gunakan `.env.example` untuk dokumentasi variabel.
- Production: gunakan Docker secrets, cloud provider secret manager (AWS Secrets Manager, Doppler), atau environment injection.

```bash
# .gitignore
.env
.env.local
.env.production
*.pem
*.key
```

## Input Validation

Validasi SEMUA input user di server-side:

### TypeScript / Hono

```ts
import { z } from "zod";

const schema = z.object({
  email: z.string().email(),
  password: z.string().min(8),
});

app.post("/register", async (c) => {
  const body = await c.req.json();
  const parsed = schema.safeParse(body);
  if (!parsed.success) {
    return c.json({ error: parsed.error }, 400);
  }
  // ...
});
```

### Go

```go
func validateEmail(email string) error {
    if _, err := mail.ParseAddress(email); err != nil {
        return errors.New("invalid email")
    }
    return nil
}
```

## SQL Injection Prevention

- **Selalu** gunakan parameterized queries / ORM.
- **Jangan** concat string SQL dengan input user.

```go
// ✅ Safe
db.Query("SELECT * FROM users WHERE email = $1", email)

// ❌ DANGEROUS
db.Query("SELECT * FROM users WHERE email = '" + email + "'")
```

## XSS Prevention

- Escape output di template engine.
- Gunakan Content Security Policy (CSP):

```
Content-Security-Policy: default-src 'self'; script-src 'self' 'unsafe-inline'
```

- Sanitize HTML jika user bisa submit rich text (DOMPurify).

## Authentication

- Gunakan **HTTP-only, Secure, SameSite=Strict** cookies.
- JWT: store di httpOnly cookie, bukan localStorage.
- Password: hash dengan bcrypt/argon2, never plaintext.
- Rate limit login endpoint (5 attempts per IP per menit).

## HTTPS

- Semua production service WAJIB HTTPS.
- Redirect HTTP → HTTPS.
- HSTS header:

```
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

## Dependency Security

- Audit dependencies secara rutin:

```bash
# JS
pnpm audit

# Go
go list -json -deps ./... | nancy sleuth

# PHP
composer audit
```

- Update critical vulnerabilities dalam 7 hari.

## CORS

- Whitelist domain, jangan allow all (`*`):

```go
// Go / Chi
r.Use(cors.Handler(cors.Options{
    AllowedOrigins: []string{"https://app.example.com"},
    AllowedMethods: []string{"GET", "POST", "PUT", "DELETE"},
    AllowedHeaders: []string{"Authorization", "Content-Type"},
    MaxAge: 300,
}))
```

## Security Headers

```
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
X-XSS-Protection: 1; mode=block
Referrer-Policy: strict-origin-when-cross-origin
Permissions-Policy: geolocation=(), microphone=()
```

## Checklist Pre-Deploy

- [ ] Tidak ada secret di repo
- [ ] Input validation di semua endpoint
- [ ] SQL injection safe
- [ ] XSS prevention aktif
- [ ] HTTPS enabled
- [ ] Security headers configured
- [ ] Rate limiting enabled
- [ ] Dependencies audited
