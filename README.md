# Практические задания 7-8 — Auth + Products API

## Установка и запуск

```bash
npm install
npm start
```

Сервер запустится на `http://localhost:3000`  
Swagger UI доступен по адресу `http://localhost:3000/api-docs`

---

## Маршруты API

### Auth

| Метод | Маршрут            | Защита | Описание                          |
|-------|--------------------|--------|-----------------------------------|
| POST  | /api/auth/register | —      | Регистрация пользователя          |
| POST  | /api/auth/login    | —      | Вход в систему, возвращает токен  |
| GET   | /api/auth/me       | 🔒 JWT | Получить текущего пользователя    |

### Products

| Метод  | Маршрут           | Защита | Описание                  |
|--------|-------------------|--------|---------------------------|
| POST   | /api/products     | —      | Создать товар             |
| GET    | /api/products     | —      | Получить список товаров   |
| GET    | /api/products/:id | 🔒 JWT | Получить товар по id      |
| PUT    | /api/products/:id | 🔒 JWT | Обновить параметры товара |
| DELETE | /api/products/:id | 🔒 JWT | Удалить товар             |

---

## Аутентификация (JWT)

После входа через `/api/auth/login` сервер возвращает `accessToken`.  
Для защищённых маршрутов необходимо передавать токен в заголовке:

```
Authorization: Bearer <ваш_токен>
```

Токен действителен **15 минут**. После истечения нужно войти заново.

---

## Примеры запросов

### Регистрация
```json
POST /api/auth/register
{
  "email": "ivan@example.com",
  "first_name": "Иван",
  "last_name": "Иванов",
  "password": "secret123"
}
```

### Вход
```json
POST /api/auth/login
{
  "email": "ivan@example.com",
  "password": "secret123"
}
```
Ответ:
```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

### Получить текущего пользователя
```
GET /api/auth/me
Authorization: Bearer <accessToken>
```

### Создать товар
```json
POST /api/products
{
  "title": "Ноутбук",
  "category": "Электроника",
  "description": "Мощный игровой ноутбук",
  "price": 120000
}
```

### Получить товар по id (требует токен)
```
GET /api/products/<id>
Authorization: Bearer <accessToken>
```

---

## Сущности

### Пользователь
| Поле           | Тип    | Описание              |
|----------------|--------|-----------------------|
| id             | string | UUID (генерируется)   |
| email          | string | Логин пользователя    |
| first_name     | string | Имя                   |
| last_name      | string | Фамилия               |
| hashedPassword | string | bcrypt-хеш пароля     |

### Товар
| Поле        | Тип    | Описание            |
|-------------|--------|---------------------|
| id          | string | UUID (генерируется) |
| title       | string | Название            |
| category    | string | Категория           |
| description | string | Описание            |
| price       | number | Цена                |