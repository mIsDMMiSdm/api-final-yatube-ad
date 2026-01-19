# API для Yatube

REST API для социальной сети Yatube, позволяющее получать доступ к постам, комментариям, группам и подпискам через HTTP-запросы.

## Описание

API Yatube предоставляет возможность:
- Просматривать посты, комментарии и группы
- Создавать, редактировать и удалять свои посты и комментарии
- Подписываться на других пользователей
- Аутентифицироваться через JWT-токены

Проект использует Django REST Framework и JWT-аутентификацию для безопасного доступа к API.

## Технологии

- Python 3.10+
- Django 3.2.16
- Django REST Framework 3.12.4
- Djoser 2.1.0
- djangorestframework-simplejwt 4.7.2

## Установка

1. Клонируйте репозиторий:
```bash
git clone https://github.com/mIsDMMiSdm/api-final-yatube-ad.git
cd api-final-yatube-ad
```

2. Создайте и активируйте виртуальное окружение:
```bash
python -m venv venv
source venv/bin/activate  # для Linux/Mac
# или
venv\Scripts\activate  # для Windows
```

3. Установите зависимости:
```bash
pip install -r requirements.txt
```

4. Выполните миграции:
```bash
cd yatube_api
python manage.py makemigrations
python manage.py migrate
```

5. Создайте суперпользователя (опционально):
```bash
python manage.py createsuperuser
```

6. Запустите сервер разработки:
```bash
python manage.py runserver
```

API будет доступно по адресу `http://127.0.0.1:8000/`

Документация API доступна по адресу `http://127.0.0.1:8000/redoc/`

## Примеры запросов к API

### Получение JWT-токена

```bash
POST http://127.0.0.1:8000/api/v1/jwt/create/
Content-Type: application/json

{
    "username": "your_username",
    "password": "your_password"
}
```

Ответ:
```json
{
    "refresh": "eyJ0eXAiOiJKV1QiLCJhbGc...",
    "access": "eyJ0eXAiOiJKV1QiLCJhbGc..."
}
```

### Получение списка постов

```bash
GET http://127.0.0.1:8000/api/v1/posts/
```

### Создание поста (требуется аутентификация)

```bash
POST http://127.0.0.1:8000/api/v1/posts/
Authorization: Bearer {your_access_token}
Content-Type: application/json

{
    "text": "Текст нового поста",
    "group": 1
}
```

### Получение комментариев к посту

```bash
GET http://127.0.0.1:8000/api/v1/posts/{post_id}/comments/
```

### Создание комментария (требуется аутентификация)

```bash
POST http://127.0.0.1:8000/api/v1/posts/{post_id}/comments/
Authorization: Bearer {your_access_token}
Content-Type: application/json

{
    "text": "Текст комментария"
}
```

### Подписка на пользователя (требуется аутентификация)

```bash
POST http://127.0.0.1:8000/api/v1/follow/
Authorization: Bearer {your_access_token}
Content-Type: application/json

{
    "following": "username_to_follow"
}
```

### Получение списка подписок (требуется аутентификация)

```bash
GET http://127.0.0.1:8000/api/v1/follow/
Authorization: Bearer {your_access_token}
```

### Поиск по подпискам

```bash
GET http://127.0.0.1:8000/api/v1/follow/?search=username
Authorization: Bearer {your_access_token}
```

## Эндпоинты API

- `GET /api/v1/posts/` - список всех постов
- `POST /api/v1/posts/` - создание нового поста (требуется аутентификация)
- `GET /api/v1/posts/{id}/` - получение конкретного поста
- `PUT/PATCH /api/v1/posts/{id}/` - обновление поста (только автор)
- `DELETE /api/v1/posts/{id}/` - удаление поста (только автор)
- `GET /api/v1/groups/` - список всех групп
- `GET /api/v1/groups/{id}/` - получение конкретной группы
- `GET /api/v1/posts/{post_id}/comments/` - список комментариев к посту
- `POST /api/v1/posts/{post_id}/comments/` - создание комментария (требуется аутентификация)
- `GET /api/v1/posts/{post_id}/comments/{id}/` - получение конкретного комментария
- `PUT/PATCH /api/v1/posts/{post_id}/comments/{id}/` - обновление комментария (только автор)
- `DELETE /api/v1/posts/{post_id}/comments/{id}/` - удаление комментария (только автор)
- `GET /api/v1/follow/` - список подписок текущего пользователя (требуется аутентификация)
- `POST /api/v1/follow/` - подписка на пользователя (требуется аутентификация)
- `POST /api/v1/jwt/create/` - получение JWT-токена
- `POST /api/v1/jwt/refresh/` - обновление access-токена
- `POST /api/v1/jwt/verify/` - проверка токена

## Права доступа

- Неаутентифицированные пользователи могут только просматривать посты, комментарии и группы (GET-запросы)
- Аутентифицированные пользователи могут создавать посты и комментарии
- Только автор может редактировать и удалять свои посты и комментарии
- Эндпоинт `/api/v1/follow/` доступен только аутентифицированным пользователям

## Автор

Проект выполнен в рамках обучения на Яндекс.Практикум
