# Nuit Lumière — восстановление проекта

Архив содержит исходники React/Vite, сервер Express/tRPC, общий код, миграции, публичные ресурсы и собранную папку `dist`. Локальный `.env`, `.git`, кэш зависимостей и `node_modules` намеренно не включены.

## Структура

- `client/src/` — React-приложение, страницы, компоненты, стили и клиентские интеграции.
- `client/public/` — изображения, видео, аудио и статические файлы.
- `server/` — Express/tRPC API, OAuth, каталог, заказы и платежные callback handlers.
- `api/[...path].ts` — serverless catch-all handler для Vercel.
- `shared/` — типы и каталог, общий для клиента и сервера.
- `drizzle/`, `supabase/` — схемы/миграции и SQL-материалы.
- `docs/` — настройки Vercel и карта эффектов.
- `dist/public/` — собранный браузерный сайт; `dist/index.js` — серверная сборка для самостоятельного запуска.

## Локальный запуск

Нужны Node.js и pnpm 10.18.0 (версия указана в `package.json`). Из корня проекта:

```sh
pnpm install --frozen-lockfile
cp .env.example .env
# Заполните только те значения, которые нужны выбранным функциям.
pnpm dev
```

Локальный frontend/API для разработки запускается проектным dev-сервером. Для проверки типов и тестов:

```sh
pnpm check
pnpm test
```

Для production-сборки и запуска Express-сервера:

```sh
pnpm build
pnpm start
```

`pnpm build` создаёт обе сборки: `dist/public/` для браузера и `dist/index.js` для самостоятельного Express runtime. В Vercel основной build output — `dist/public`, а API обслуживается `api/[...path].ts`.

## Vercel

В корне уже есть `vercel.json`. Подключите этот каталог как проект, оставьте корень проекта без дополнительной Root Directory, используйте команды и output, заданные в `vercel.json`, и добавьте требуемые environment variables в Vercel Project Settings. После добавления/изменения переменных выполните новый deployment.

Шаблон `.env.example` содержит только безопасные placeholders, не рабочие credentials. Настоящий MySQL-compatible `DATABASE_URL`, Manus OAuth configuration и JWT secret нужны для server-backed функций; дополнительные секреты требуются только для соответствующих integrations. См. `docs/vercel-environment.md`. Не загружайте `.env` в ZIP, чат, Git или клиентский bundle. Для локальной работы заполняйте `.env` на своей машине; в Vercel храните переменные в настройках проекта.

Без секретов и настроенной базы статический интерфейс и встроенный seed-каталог могут отображаться, но серверные функции — авторизация, администрирование, сохранение заказов и оплата — не будут полностью работать.

## Эффекты

См. [`docs/effects-restoration.md`](docs/effects-restoration.md) — там указаны исходные файлы, точки подключения и внешние зависимости typewriter, Golden Dust, Oracle Search, Mirror scroll, звуков и голоса. Папка `dist/` — артефакт сборки, не заменяющий эти читаемые исходники.
