# Teach Space

## backend

1. Установить Docker Desktop
2. Установка Docker compose (если не ставится с Docker Desktop)
3. Запуск контейнеров с сервером, postgress и pgadmin
   Запустить Docker Desktop, затем выполнить команды:

```sh
cd backend
docker-compose up --build
```

4. Swager будет доступен по адресу http://localhost:8000/docs или http://0.0.0.0:8000/docs

5. Для того, чтобы завершить работу ввести команду

```sh
docker-compose down --volumes
```

## frontend

```sh
cd frontend
yarn dev
=======
# Teach-Space Frontend

Фронтенд-приложение **Teach-Space** — образовательная платформа, построенная на React + Vite с использованием TypeScript.

## Стек технологий

- **React 19** — UI-библиотека
- **Vite** — сборщик и dev-сервер
- **Redux Toolkit** — управление состоянием
- **React Router DOM** — маршрутизация
- **Material UI (MUI)** — компоненты интерфейса
- **Tailwind CSS** — утилитарные стили
- **Sass** — CSS-препроцессор
- **TypeScript** — типизация
- **ESLint** — линтинг

## Установка и запуск

### 1. Установка зависимостей

```bash
npm install
```

### 2. Запуск dev-сервера

```bash
npm run dev
```

После запуска откройте браузер по адресу, указанному в терминале (обычно `http://localhost:5173`).

## Доступные команды

| Команда           | Описание                                   |
| ----------------- | ------------------------------------------ |
| `npm run dev`     | Запуск dev-сервера с горячей перезагрузкой |
| `npm run build`   | Сборка проекта для продакшена              |
| `npm run preview` | Предпросмотр продакшен-сборки              |
| `npm run lint`    | Проверка кода линтером ESLint              |


## CI/CD

Деплой в AWS настроен через workflow [.github/workflows/aws.yml](/home/pavel/work/Teach-Space/.github/workflows/aws.yml).

### Test / staging

- Деплой запускается при `push` в ветку `main`.
- Workflow собирает Docker-образ, публикует его в Amazon ECR с тегом `${{ github.sha }}` и обновляет тестовый ECS-сервис.
- Для выкладки используется текущее определение задачи из `ECS_TASK_DEFINITION_STAGING`, после чего регистрируется новая ревизия task definition с новым образом.
- Сервис обновляется через `aws ecs update-service`, затем pipeline ждёт состояния `services-stable`.

### Production

- Деплой в production запускается при пуше git-тега формата `v*.*.*`, например `v1.2.3`.
- Docker-образ публикуется в Amazon ECR с тегом релиза `${{ github.ref_name }}`.
- После этого workflow обновляет production ECS-сервис, используя `ECS_TASK_DEFINITION_PRODUCTION`, `ECS_CLUSTER_PRODUCTION` и `ECS_SERVICE_PRODUCTION`.
- Pipeline также ждёт стабилизации сервиса перед завершением job.

### Необходимые GitHub secrets и variables

- `secrets.AWS_ACCESS_KEY_ID`
- `secrets.AWS_SECRET_ACCESS_KEY`
- `secrets.ECR_REPOSITORY`
- `vars.AWS_REGION`
- `vars.ECS_TASK_DEFINITION_STAGING`
- `vars.ECS_CLUSTER_STAGING`
- `vars.ECS_SERVICE_STAGING`
- `vars.ECS_TASK_DEFINITION_PRODUCTION`
- `vars.ECS_CLUSTER_PRODUCTION`
- `vars.ECS_SERVICE_PRODUCTION`

## Структура проекта

```
frontend/
├── public/          # Статические файлы
├── src/
│   ├── components/  # React-компоненты
│   ├── App.tsx      # Корневой компонент
│   ├── main.tsx     # Точка входа
│   └── styles.css   # Глобальные стили
├── index.html       # HTML-шаблон
└── package.json     # Зависимости и скрипты
```