---
title: "Flutter Development Standards"
description: "Standar development untuk Flutter apps."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["flutter", "dart", "mobile"]
isPinned: false
growthStage: "seedling"
---

## Architecture

Gunakan **Clean Architecture** dengan BLoC (Business Logic Component) untuk state management.

```
lib/
├── core/                 # Shared utilities, theme, constants
├── features/
│   └── auth/
│       ├── data/         # Repositories, models, datasources
│       ├── domain/       # Entities, use cases
│       └── presentation/ # BLoC, screens, widgets
├── main.dart
└── injection.dart        # GetIt DI setup
```

## State Management

Gunakan **flutter_bloc** (Cubit untuk simple, Bloc untuk complex):

```dart
class AuthCubit extends Cubit<AuthState> {
    AuthCubit(this._repo) : super(AuthInitial());

    final AuthRepository _repo;

    Future<void> login(String email, String password) async {
        emit(AuthLoading());
        try {
            final user = await _repo.login(email, password);
            emit(AuthSuccess(user));
        } catch (e) {
            emit(AuthError(e.toString()));
        }
    }
}
```

## Navigation

Gunakan **GoRouter** untuk declarative routing:

```dart
final router = GoRouter(
    routes: [
        GoRoute(path: '/', builder: (_, __) => HomeScreen()),
        GoRoute(path: '/login', builder: (_, __) => LoginScreen()),
    ],
);
```

## Dependency Injection

Gunakan **GetIt** untuk DI:

```dart
final getIt = GetIt.instance;

void setupDependencies() {
    getIt.registerSingleton<AuthRepository>(AuthRepositoryImpl());
    getIt.registerFactory(() => AuthCubit(getIt()));
}
```

## Testing

```bash
# Unit test
flutter test

# Integration test
flutter test integration_test/
```

## Code Quality

```bash
# Format
dart format .

# Analyze
dart analyze

# Linter (di pubspec.yaml)
analyzer:
    enable-experiment:
        - records
        - patterns
```
