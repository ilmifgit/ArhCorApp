# Отчёт по заданию №4: Проектирование API

- **Студент:** `Илья Митрофанов`
- **Система:**` D&D Character Manager`
- **Базовый URL:** `localhost`

---

## Список всех HTTP-методов, используемых в API

| Метод    | Назначение                     |
|----------|--------------------------------|
| `POST`   | Создание ресурса (регистрация, логин, создание персонажа) |
| `GET`    | Получение данных (список персонажей, детали) |
| `PUT`    | Полное обновление ресурса (редактирование персонажа) |
| `DELETE` | Удаление ресурса (удаление персонажа) |

---

## 1. Регистрация пользователя
- **Метод:** `POST`
- **URL:** `/auth/register`
- **Заголовки:** `Content-Type: application/json`
- **Тело запроса:**
  ```json
  {
    "username": "MageKiller",
    "email": "player@example.com",
    "password": "secure123"
  }
  ```
 **Успешный ответ (201 Created):**
```json
{
  "user_id": 1,
  "username": "MageKiller",
  "email": "player@example.com",
  "message": "User registered successfully"
}
```
**Ошибки: 400, 409, 500**
## 2. Авторизация (логин)
- **Метод:** `POST`
- **URL:** `/auth/login`
- **Тело запроса:**
```json
{
  "username": "MageKiller",
  "password": "secure123"
}
```
**Успешный ответ (200 OK):**
```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIs...",
  "token_type": "Bearer",
  "expires_in": 3600,
  "user_id": 1,
  "username": "MageKiller"
}
```
**Ошибки: 401, 400**
## 3. Получение списка персонажей
- **Метод:** `GET`
- **URL:** `/characters`
- **Заголовки:** `Authorization: Bearer <token>`
- **Параметры:** `page, limit (опционально)`
**Успешный ответ (200 OK):**
```json
{
  "items": [ { "id": 101, "name": "Aragorn", "race": "Человек", ... } ],
  "total": 2,
  "page": 1,
  "limit": 10
}
```
**Ошибки: 401, 500**
## 4. Создание нового персонажа
- **Метод:** `POST`
- **URL:** `/characters`
- **Заголовки:** `Authorization: Bearer <token>, Content-Type: application/json`
- **Тело запроса:**
```json
{
  "name": "Gandalf",
  "race": "Майар",
  "subrace": "Истари",
  "class": "Волшебник",
  "subclass": "Школа иллюзий",
  "trait": "Мудрость веков",
  "background": "Таинственный советник"
}
```
**Успешный ответ (201 Created):**
```json
{
  "id": 103,
  "name": "Gandalf",
  "race": "Майар",
  "subrace": "Истари",
  "class": "Волшебник",
  "subclass": "Школа иллюзий",
  "trait": "Мудрость веков",
  "background": "Таинственный советник",
  "user_id": 1,
  "message": "Character created"
}
```
**Ошибки: 400, 401, 409**
## 5. Просмотр деталей персонажа
- **Метод:** `GET`
- **URL:** `/characters/{character_id}`
- **Заголовки:** `Authorization: Bearer <token>`
**Успешный ответ (200 OK):**
```json
{
  "id": 103,
  "name": "Gandalf",
  "race": "Майар",
  "subrace": "Истари",
  "class": "Волшебник",
  "subclass": "Школа иллюзий",
  "trait": "Мудрость веков",
  "background": "Таинственный советник",
  "user_id": 1,
  "created_at": "2026-05-31T10:00:00Z"
}
```
**Ошибки: 401, 403, 404**
## 6. Редактирование персонажа
- **Метод:** `PUT`
- **URL:** `/characters/{character_id}`
- **Заголовки:** `Authorization: Bearer <token>, Content-Type: application/json`
- **Тело запроса (все поля):**
```json
{
  "name": "Gandalf the White",
  "race": "Майар",
  "subrace": "Истари",
  "class": "Волшебник",
  "subclass": "Школа преобразования",
  "trait": "Бессмертие",
  "background": "Таинственный советник"
}
```
**Успешный ответ (200 OK):**
```json
{
  "id": 103,
  "name": "Gandalf the White",
  "race": "Майар",
  "subrace": "Истари",
  "class": "Волшебник",
  "subclass": "Школа преобразования",
  "trait": "Бессмертие",
  "background": "Таинственный советник",
  "message": "Character updated"
}
```
**Ошибки: 400, 401, 403, 404**
## 7. Удаление персонажа
- **Метод:** `DELETE`
- **URL:** `/characters/{character_id}`
- **Заголовки:** `Authorization: Bearer <token>`
- **Успешный ответ:** `204 No Content`
**Ошибки: 401, 403, 404**

