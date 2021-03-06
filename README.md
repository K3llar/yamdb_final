# YAMDB

## В данном проекте представлен программный интерфейс приложения (API) для работы с сервисом YamBD.

Сервис YamBD служит для сбора отзывов пользователей на произведения различных категорий.
Новые категории могут быть добавлены по решению администратора сервиса.
Каждому произведению может быть присвоен жанр из списка предустановленных администратором.
Пользователь в своем отзыве может указать оценку по десятибальной шкале на основе которой будет вычисляться общий рейтинг произведения.

В разработке API участвовали студенты: Бутаков Александр и Ковалевский Никита

## Инструкция для запуска приложения
Для запуска необходимо добавить SECRETS в github.workflows

- ```DOCKER_USERNAME``` - имя пользователя в hub.docker
- ```DOCKER_PASSWORD``` - пароль
- ```HOST``` - адрес сервера
- ```USER``` - пользователь
- ```SSH_KEY``` - приватный ssh ключ
- ```PASSPHRASE``` - кодовая фраза
- ```DB_ENGINE``` - django.db.backends.postgresql
- ```DB_NAME``` - postgres (по умолчанию)
- ```POSTGRES_USER``` - postgres (по умолчанию)
- ```POSTGRES_PASSWORD``` - postgres (по умолчанию)
- ```DB_HOST``` - db
- ```DB_PORT``` - 5432
- ```SECRET_KEY``` - секретный ключ приложения django (необходимо чтобы были экранированы или отсутствовали скобки)
- ```ALLOWED_HOSTS``` - список разрешенных адресов
- ```TELEGRAM_TO``` - id пользователя
- ```TELEGRAM_TOKEN``` - токен бота

### Для запуска проекта необходимо выполнить следующие шаги

```
git clone https://github.com/K3llar/yamdb_final.git
cd yamdb_final
```

#### При первом запуске необходимо выполнить следующие команды:
Для проведения необходимых миграций в базе данных 
```
docker-compose exec backend python manage.py makemigrations
docker-compose exec backend python manage.py migrate
```
Для подключения статических файлов
```
docker-compose exec backend python manage.py collectstatic --no-input
```

Для создания суперпользователя
```
docker-compose exec backend python manage.py createsuperuser
```

## Пользовательские роли и разрешения:

***Аноним*** - может просматривать описания произведений, читать отзывы и комментарии к ним.

***Аутентифицированный пользователь*** - может читать всё, как и Аноним, может публиковать отзывы и ставить оценки произведениям (фильмам/книгам/песенкам), может комментировать отзывы; может редактировать и удалять свои отзывы и комментарии, редактировать свои оценки произведений. Эта роль присваивается по умолчанию каждому новому пользователю.

***Модератор*** - те же права, что и у Аутентифицированного пользователя, плюс право удалять и редактировать любые отзывы и комментарии.

***Администратор*** - полные права на управление всем контентом проекта. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.


## Регистрация новых пользователей:

1. Пользователь отправляет POST-запрос с параметрами email и username на эндпоинт /api/v1/auth/signup/.

2. Сервис YaMDB отправляет письмо с кодом подтверждения (confirmation_code) на указанный адрес email.

3. Пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит token (JWT-токен).

4. В результате пользователь получает токен и может работать с API проекта, отправляя этот токен с каждым запросом.

После регистрации и получения токена пользователь может отправить PATCH-запрос на эндпоинт /api/v1/users/me/ и заполнить поля в своём профайле (описание полей — в документации).

## Работа с приложением:

Получение списка всех категорий: [GET]: /api/v1/categories/

```
EXAMPLE ANSWER

[
  {
    "count": 0,
    "next": "string",
    "previous": "string",
    "results": [
      {
        "name": "string",
        "slug": "string"
      }
    ]   
  }
]
```


Получение списка всех жанров: [GET]: /api/v1/genres/

```
EXAMPLE ANSWER

[
  {
    "count": 0,
    "next": "string",
    "previous": "string",
    "results": [
      {
        "name": "string",
        "slug": "string"
      }
    ]   
  }
]
```

Получение списка всех произведения: [GET]: /api/v1/titles

```
EXAMPLE ANSWER

[
  {
    "count": 0,
    "next": "string",
    "previous": "string",
    "results": [
      {
            "id": 0,
            "name": "string",
            "year": 0,
            "rating": 0,
            "description": "string",
            "genre": [
              {
                "name": "string",
                "slug": "string"
              }
            ],
            "category": {
                "name": "string",
                "slug": "string"
             }
          }
       ]   
     }
]
```


Получение информации о произведении: [GET]: api/v1/titles/{titles_id:int}/

```
EXAMPLE ANSWER

[
  {
    "id": 0,
    "name": "string",
    "year": 0,
    "rating": 0,
    "description": "string",
    "genre": [
      {
      "name": "string",
      "slug": "string"
      }
    ],
    "category": {
       "name": "string",
       "slug": "string"
   }
}
```

### Таким же образом можно получить списки и конкретных экземпляров классов:

Отзывы [GET]: /api/v1/titles/{titles_id:int}/reviews/{review_id:int}/

Комментарии [GET]: /api/v1/titles/{titles_id:int}/reviews/{review_id:int}/comments/{comment_id:int}/

Пользователи [GET]: /api/v1/users/{user_id:int}/


Более подробная информация представлена на [GET]: /redoc/

![workflow](https://github.com/K3llar/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)
