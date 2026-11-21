# BookStack

Краткое описание: BookStack - веб-платформа для ведения документации и базы знаний. Контент организован в книги, главы и страницы, а доступ управляется ролями и правами. В этой версии русский язык задан как язык приложения по умолчанию.

## ✨ Возможности

- Иерархия документации: полки, книги, главы и страницы.
- Визуальный редактор и Markdown-редактор.
- Поиск по контенту.
- Роли, разрешения и ограничения доступа.
- Вложения, изображения и экспорт.
- Аутентификация через встроенные учетные записи и внешние провайдеры.
- Русские языковые файлы находятся в `lang/ru`.

## 🚀 Быстрый старт

```bash
git clone https://github.com/QuestGuideRF/BookStack.git
cd BookStack
composer install
npm install
cp .env.example .env
php artisan key:generate
php artisan migrate
npm run build
php artisan serve
```

Для запуска через Docker Compose:

```bash
docker compose up
```

По умолчанию dev-контейнер публикует приложение на `http://localhost:8080`, если переменная `DEV_PORT` не переопределена.

## 📦 Установка зависимостей

```bash
composer install
npm install
```

Минимальная версия PHP задана в `composer.json`: PHP `^8.2.0`. JavaScript-зависимости и скрипты сборки описаны в `package.json`.

## 📁 Структура проекта

```text
BookStack/
├─ app/                         # Laravel-приложение и доменная логика
├─ bootstrap/                   # загрузка Laravel
├─ database/                    # миграции, фабрики и сидеры
├─ lang/                        # переводы интерфейса
├─ public/                      # публичные файлы и собранные ассеты
├─ resources/                   # шаблоны, Sass и JavaScript
├─ routes/                      # маршруты приложения
├─ storage/                     # кэш, логи и пользовательские файлы
├─ tests/                       # PHPUnit-тесты
├─ themes/                      # пользовательские темы
├─ composer.json                # PHP-зависимости и команды
├─ package.json                 # npm-зависимости и команды
└─ docker-compose.yml           # локальный Docker-запуск
```

## 🛠 Использование

После установки настройте `.env`, выполните миграции и запустите сервер. В браузере откройте адрес, указанный в `APP_URL`, или адрес локального сервера Artisan.

Полезные команды:

```bash
php artisan migrate
npm run build
npm run dev
composer test
npm test
```

## ⚙️ Конфигурация

- Основные переменные окружения находятся в `.env.example` и `.env.example.complete`.
- `APP_LANG=ru` добавлен в `.env.example` и `docker-compose.yml`.
- Значения `locale`, `default_locale`, `fallback_locale` и `faker_locale` настроены в `app/Config/app.php`.
- PHPUnit настроен через `phpunit.xml`.
- Jest настроен через `jest.config.ts`.
- TypeScript и ESLint настроены через `tsconfig.json` и `eslint.config.mjs`.

## 🧪 Тестирование

```bash
composer test
npm test
```

Дополнительные проверки:

```bash
composer lint
npm run lint
npm run ts:lint
```

## 🧰 Технологии

- PHP 8.2 и Laravel 12.
- Composer.
- Node.js, npm, TypeScript и Sass.
- PHPUnit и Jest.
- MySQL/MariaDB.
- Docker Compose для локального окружения.

## 📝 Лицензия

Условия лицензии MIT опубликованы в [исходном репозитории BookStack](https://github.com/BookStackApp/BookStack).

## Разработка и тесты

PHP-код находится в `app/`, Blade-шаблоны — в `resources/views/`, стили и JavaScript — в `resources/`. Ветка `development` служит основой для разработки и подготовки выпуска.

| Задача | Команда |
| --- | --- |
| Собрать фронтенд | `npm run build` |
| Собрать и минифицировать фронтенд для релиза | `npm run production` |
| Пересобирать фронтенд при изменениях | `npm run dev` |
| Проверить PHP-стиль и типы | `composer lint`, `composer check-static` |
| Проверить JavaScript и TypeScript | `npm run lint`, `npm run ts:lint` |
| Выполнить функциональные тесты PHP | `composer test` или `vendor/bin/phpunit` |
| Выполнить тесты JavaScript | `npm test` |

Тестам PHP нужна отдельная база с настройками из `.env.testing`. Для запуска части набора передайте PHPUnit путь к файлу или `--filter <имя>`. Тесты редактора используют Jest и вспомогательные функции рядом с исходниками WYSIWYG. Сценарий `dev/docker/db-testing/run.sh` выполняется из своего каталога и прогоняет проверки по поддерживаемым версиям MySQL и MariaDB. Аргумент может задавать ветку или тег, например `./run.sh v25-11`.

При проверке прав учитывайте прямое разрешение, наследование, владение сущностью и конфликт между ролями. Классы тестов разделяют сценарии общих разрешений роли и разрешений на отдельных объектах.

### Компоненты JavaScript и редактор

Компоненты регистрируются в загрузчике BookStack и привязываются к HTML через `component`. Локальные элементы и параметры задаются атрибутами `refs` и `option:<компонент>:<параметр>`. Исходники и тесты редактора находятся в `resources/js/wysiwyg/`.

Публичные события отправляются через DOM. Обработчик регистрируют через `window.addEventListener(...)`, параметры читают из `event.detail`.

| Событие | Назначение |
| --- | --- |
| `editor-markdown-cm6::pre-init` | Настройка Markdown-редактора на CodeMirror 6 до инициализации |
| `editor-markdown::setup` | Подключение к уже созданному Markdown-редактору |
| `editor-drawio::configure` | Настройка параметров интеграции draw.io |
| `editor-tinymce::pre-init` / `editor-tinymce::setup` | Настройка TinyMCE до и при инициализации |
| `editor-wysiwyg::post-init` | Доступ к API визуального редактора после инициализации |
| `library-cm6::configure-theme` / `library-cm6::pre-init` / `library-cm6::post-init` | Настройка темы и подключение к редакторам CodeMirror 6 |

В API WYSIWYG модуль `ui` предоставляет `createButton(options)` и доступ к основной панели через `getMainToolbar()`. В модуле `content` метод `insertHtml(html, position)` добавляет фрагмент HTML. Интерфейс развивается, поэтому имена и параметры сверяйте с исходниками. Переводы TinyMCE размещаются в `public/libs/tinymce/langs/` и поступают из [проекта переводов TinyMCE](https://crowdin.com/project/tinymce).

### Темы и модули

Укажите тему через `APP_THEME`. В `themes/<имя>/` можно переопределять `views/`, `icons/` и `lang/`. Файлы из `public/` темы доступны клиентам, а `functions.php` расширяет поведение через фасад `Theme`. Основные методы фасада — `Theme::listen()`, `Theme::addSocialDriver()` и `Theme::registerCommand()`. Локальные правки отдельных представлений и расширения фасада следует пересматривать при обновлении BookStack.

Модуль помещается в `themes/<имя>/modules/<модуль>/` и содержит обязательный `bookstack-module.json` со строками `name`, `description` и `version`. Дополнительно поддерживаются `functions.php` и каталоги `head/`, `views/`, `icons/`, `lang/`, `public/`. При конфликте переопределений активная тема имеет приоритет. Порядок применения разных модулей не гарантируется.

### Переносимый ZIP

Экспорт содержит `data.json` и каталог `files/` с приложенными объектами. Это формат обмена страницами, главами или книгами. Для резервного копирования экземпляра нужны также БД и данные приложения.

- В JSON могут присутствовать `instance` с полями `id` и `version`, `exported_at` в ISO 8601 и объект `book`, `chapter` или `page`.
- Книга и глава содержат `name`, необязательные `id`, `description_html` и списки дочерних сущностей. Общий порядок страниц и глав внутри книги определяется `priority`.
- Страница содержит `name` и `html` или `markdown`. Непустой Markdown задаёт тип редактора. Изображения могут иметь тип `gallery` или `drawio`.
- Ссылки на объекты используют форму `[[bsexport:page:40]]`; аналогично указывают `book`, `chapter`, `attachment` и другие поддерживаемые типы. HTML из импортируемых файлов обрабатывается как внешнее содержимое.

### Выпуск версии

Релизы используют формат `v<год>-<месяц>` с отдельными исправлениями по необходимости. Перед публикацией подготавливают заметки к выпуску, зависимости и собранные веб-ресурсы, затем переносят изменения из `development` в релизную ветку.

### Происхождение редактора

BookStack использует адаптированные компоненты Lexical v0.17.1. Сведения об авторских правах и исходной лицензии приведены рядом с [кодом Lexical](resources/js/wysiwyg/lexical/readme.md).

Подпись относится к текущей версии проекта.

---

Автор: legenda  
Telegram: [@LEGENDA_SD](https://t.me/LEGENDA_SD)
