---
title: "NestJS Development Standards"
description: "Standar development untuk NestJS backend."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["nestjs", "typescript", "backend"]
isPinned: false
growthStage: "seedling"
---

## Project Layout

```
project/
├── src/
│   ├── main.ts               # Entry point
│   ├── app.module.ts         # Root module
│   ├── modules/
│   │   └── users/
│   │       ├── users.module.ts
│   │       ├── users.controller.ts
│   │       ├── users.service.ts
│   │       ├── users.repository.ts
│   │       └── dto/
│   │           ├── create-user.dto.ts
│   │           └── update-user.dto.ts
│   ├── common/               # Filters, interceptors, pipes, guards
│   └── config/               # App config
├── test/
├── package.json
└── nest-cli.json
```

## Module Pattern

```ts
// users.module.ts
@Module({
  controllers: [UsersController],
  providers: [UsersService, UsersRepository],
  exports: [UsersService],
})
export class UsersModule {}
```

## Controller

```ts
@Controller("users")
export class UsersController {
  constructor(private readonly service: UsersService) {}

  @Get()
  async findAll() {
    return this.service.findAll();
  }

  @Post()
  async create(@Body() dto: CreateUserDto) {
    return this.service.create(dto);
  }
}
```

## DTO Validation

Gunakan **class-validator** + **class-transformer**:

```ts
import { IsString, IsEmail, MinLength } from "class-validator";

export class CreateUserDto {
  @IsString()
  @MinLength(2)
  name: string;

  @IsEmail()
  email: string;
}
```

## Database

Gunakan **Prisma** atau **TypeORM**:

```ts
// Prisma schema
model User {
    id    Int     @id @default(autoincrement())
    name  String
    email String  @unique
}
```

## Testing

```bash
# Unit test
pnpm test

# E2E
pnpm test:e2e
```

```ts
// Controller unit test
describe("UsersController", () => {
  let controller: UsersController;
  let service: UsersService;

  beforeEach(async () => {
    const module = await Test.createTestingModule({
      controllers: [UsersController],
      providers: [{ provide: UsersService, useValue: mockService }],
    }).compile();

    controller = module.get(UsersController);
  });

  it("should return all users", async () => {
    expect(await controller.findAll()).toEqual(mockUsers);
  });
});
```

## Deployment

Build dan run:

```bash
pnpm build
node dist/main.js
```

Atau dengan Docker:

```dockerfile
FROM node:22-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build
CMD ["node", "dist/main"]
```
