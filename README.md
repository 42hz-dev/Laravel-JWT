# 🔐 Laravel JWT Practice

JWT(JSON Web Token)를 사용한 인증 기능 연습 프로젝트입니다.

---

## 📦 설치 단계

### ✅ Step 1: 패키지 설치
```bash
composer require php-open-source-saver/jwt-auth
```

### ✅ Step 2: JWT 시크릿 키 생성
```bash
sail artisan jwt:secret
```

### ✅ Step 3: `bootstrap/app.php` 수정

```php
return Application::configure(basePath: dirname(__DIR__))
    ->withRouting(
        web: __DIR__.'/../routes/web.php',
        api: __DIR__.'/../routes/api.php',
        commands: __DIR__.'/../routes/console.php',
        health: '/up',
    )
    ->withMiddleware(function (Middleware $middleware) {
        //
    })
    ->withExceptions(function (Exceptions $exceptions) {
        //
    })->create();
```

### ✅ Step 4: `config/auth.php` 수정
```php
'defaults' => [
    'guard' => env('AUTH_GUARD', 'api'),
    'passwords' => env('AUTH_PASSWORD_BROKER', 'users'),
],

'guards' => [
        'web' => [
            'driver' => 'session',
            'provider' => 'users',
        ],
        'api' => [
            'driver' => 'jwt',
            'provider' => 'users',
        ],
    ],
```

### ✅ Step 5: `Models/User.php` 추가
```php
class User extends Authenticatable implements JWTSubject
{
public function getJWTIdentifier(): mixed
    {
        // TODO: Implement getJWTIdentifier() method.
        return $this->getKey();
    }

    public function getJWTCustomClaims(): array
    {
        // TODO: Implement getJWTCustomClaims() method.
        return [];
    }
}
```
### ✅ Step 6: `.env` 추가
```php
AUTH_GARD=api
```

---

## 🚀 API 사용 방법

### 🔹 회원가입

- **URL**: `/api/register`
- **Method**: `POST`
- **Headers**:
  ```
  Accept: application/json
  ```
- **Body (JSON)**:
```json
{
  "name": "admin",
  "email": "admin@admin.com",
  "password": "admin1234"
}
```

---

### 🔹 로그인

- **URL**: `/api/login`
- **Method**: `POST`
- **Headers**:
  ```
  Accept: application/json
  ```
- **Body (JSON)**:
```json
{
  "email": "admin@admin.com",
  "password": "admin1234"
}
```

---

### 🔹 로그아웃

- **URL**: `/api/logout`
- **Method**: `POST`
- **Headers**:
  ```
  Accept: application/json  
  Authorization: Bearer {token}
  ```

---

### 🔹 토큰 갱신 (Refresh)

- **URL**: `/api/refresh`
- **Method**: `POST`
- **Headers**:
  ```
  Accept: application/json  
  Authorization: Bearer {token}
  ```

---

## 🧾 참고

- JWT 토큰은 로그인 성공 시 응답의 `authorisation.token` 항목에 포함됩니다.
- 클라이언트는 이 토큰을 저장하고 이후 요청 시 `Authorization` 헤더에 포함시켜야 합니다.
- `Accept: application/json` 헤더는 Laravel이 JSON 응답을 반환하게 합니다.
