**[Demo](http://84.38.180.229:86)**

**Учетные данные для входа в систему**

Суперпользователь

логин: `admin@domain.com`  
пароль: 1

Сотрудник

логин: `user@mail.com`  
пароль: 1

Пример файла с реестром книг для загрузки - `books.xlsx`

## Используемые технологии

- Laravel

- jQuery

- Bootstrap

- Docker

## Задача

Необходимо создать небольшую систему управления библиотекой. Сотрудникам необходимо иметь реестр всех книг. Все книги распределены по категориям(секциям) библиотеки

Каждая книга имеет следующую информацию: [title, slug, author, description, rating, cover(картинка)]. Каждая категория: [title, slug]

**Функционал**

- Авторизация для сотрудников

- Регистрация для читателей

- Создайте CRUD для книг, категорий, сотрудников

- Страницы просмотра книги

- Страница с просмотром списка книг. Используйте пагинацию

**Минимальные требования**

- Используйте Laravel

- Все обложки должны сохраняться в папке storage/app/public/covers

- Используйте Request классы

- Используйте seeds для создания первого пользователя с помощью (admin@domain.com password) и заполнения таблиц тестовыми данными.
Для картинок можете использовать сервис placeholder.com или любую готовую картинку.

- Используйте миграции

- Используйте отношения

**Продвинутые задачи**

- Реализуйте email уведомление при добавлении нового сотрудника

- Добавьте возможность читателям оставлять комментарии к книгам

- Реализуйте API
  
- Реализуйте парсер excel файла с реестром книг через job по 100 книг за раз. Можете использовать maatwebsite/excel

## Реализовано

- При первоначальной загрузке (seed) создается суперпользователь (логин: admin@domain.com; пароль: 1), который может создавать сотрудников (меню Действия -> Добавить сотрудника). Роль суперпользователя - 0. У создаваемых сотрудников роль - 1. Сотрудники могут создавать/удалять/редактировать книги и категории, также могут загружать книги из .xlsx-файла.

- Авторизация для сотрудников. Созданные суперпользователем сотрудники получают email с логином и паролем, после чего могут входить в систему со своими правами.

- Регистрация читателей. Читатели регистрируются, после чего могут просматривать книги, выполнять поиск, фильтрацию по категориям.

- CRUD для книг, категорий, сотрудников.

- Просмотр книг.

- Комментарии книг.

- Пагинация.

- Загрузка обложек книг. Сохраняются в /storage/app/public/cover

- Первоначальная загрузка данных (seeding).

- email уведомление сотрудников при их создании суперпользователем (сотруднику отправляется email с логином и паролем).

- Загрузка книг из .xlsx-файла по 100 записей в job-е (меню Действия -> Импортировать книги). Загрузка книг доступна суперпользователю и сотрудникам.

- API для книг. 
  
  **Endpoints:**

  `GET /api/books` - получение всех книг
  
  `POST /api/books` - создание книги
  
  ``` 
  {
    "title":"book title",
    "author":"book author",
    "description":"book description",
    "rating":4,
    "created_at":"2023-05-18 22:09:36",
    "category_id":2
  }
  ```

  `POST /api/books/15` - изменение книги с id=15

  ```
  {
    "_method": "PUT",
    "title": "changed title"
  }
  ```

  `POST /api/books/15` - удаление книги с id=15

  ```
  {
    "_method": "DELETE"
  }
  ```

## Запуск проекта

Переименовать файл **.env.example** в **.env**.

Для отправки почты нужно зарегистрировать приложение на каком-либо почтовом сервисе (в файле **.env** настройки для mail.ru), получить [пароль для внешних приложений](https://help.mail.ru/mail/security/protection/external) и добавить его в параметр `MAIL_PASSWORD` в файле **.env**. В параметрах `MAIL_USERNAME` и `MAIL_FROM_ADDRESS` указать свой email.

### Локально

Требуется PHP версии >= 8.1 с установленным zip extension, Composer.

- Перед запуском приложения необходимо создать базу данных MySQL с именем **library**, пользователем **root** без пароля

- Клонировать проект

  `git clone git@github.com:ignal1/library-laravel.git`

- В файле **.env** раскомментировать следующие настройки для БД

    ```
    DB_CONNECTION=mysql
    DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE=library
    DB_USERNAME=root
    DB_PASSWORD=
    ```

- В Bash выполнить команды

  `composer install`

  `php artisan migrate:fresh --seed`

  `php artisan storage:link`

  `php artisan queue:work`

  `php artisan serve`

  после чего приложение будет доступно на *http://localhost:8000*

### Docker

- В файле **.env** раскомментировать следующие настройки для БД

    ```
    DB_CONNECTION=mysql
    DB_HOST=127.0.0.1
    DB_PORT=33061
    DB_DATABASE=app
    DB_USERNAME=app
    DB_PASSWORD=secret
    ```
  
- В файле **script.js** на строке 44 изменить *127.0.0.1:8000* на *localhost:8080*
  
- Перейти в директорию проекта и выполнить команды:

  `composer install`

  `docker-compose up -d`

  `docker-compose exec php-cli php artisan migrate:fresh --seed`
  
  `docker-compose exec php-cli php artisan storage:link`
  
  `docker-compose exec php-cli php artisan queue:work`

  после чего приложение будет доступно на *http://localhost:8080*


