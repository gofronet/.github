# Gofro Platform

Gofro — платформа управления Xray-нодами.

Она разделена на три независимых компонента:
- `gofro-control` — control-plane API;
- `gofro-node` — data-plane агент на ноде;
- `gofro-web` — операторская веб-панель.

Проект решает задачу централизованного управления нодами: подключение, наблюдение состояния, обновление конфигурации и управление жизненным циклом Xray.

## Что входит в MVP

- централизованная точка управления нодами через HTTP API;
- удаленное выполнение команд на ноде через gRPC;
- UI для базовых операторских действий;
- единый контракт операций: `start/stop/restart`, `get/update config`, `get node info`.

## Архитектура

```text
Browser (gofro-web)
        |
        | HTTP REST
        v
  gofro-control
        |
        | gRPC
        v
   gofro-node
        |
        | local process/files
        v
    Xray Core
```

## Границы ответственности

- `gofro-web` работает только с `gofro-control`.
- `gofro-control` не управляет Xray напрямую, а делегирует команды в `gofro-node`.
- `gofro-node` выполняет команды локально и управляет процессом Xray на своей машине.

## Базовый сценарий эксплуатации

1. Запустить `gofro-node` на целевом сервере.
2. Запустить `gofro-control`.
3. Зарегистрировать ноду в control-plane через `POST /v1/nodes/`.
4. Работать через API или `gofro-web`.

## Локальный запуск стенда

1. Поднять `gofro-node` (по умолчанию gRPC `:50051`).
2. Поднять `gofro-control` (по умолчанию HTTP `:8080`).
3. Добавить ноду `127.0.0.1:50051` в control-plane.
4. Поднять `gofro-web` с `API_BASE_URL=http://localhost:8080`.

## Текущие ограничения

- список нод в `gofro-control` хранится в памяти процесса;
- нет полноценной authN/authZ модели между компонентами;
- gRPC между `gofro-control` и `gofro-node` работает без TLS/mTLS;

## Документы компонентов

- `README-gofro-control.md`
- `README-gofro-node.md`
- `README-gofro-web.md`

## Репозитории компонентов

- `gofro-control`: `<добавьте URL репозитория>`
- `gofro-node`: `<добавьте URL репозитория>`
- `gofro-web`: `<добавьте URL репозитория>`
