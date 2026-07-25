# Orchestrator — обзор

## Ответственность

`LocalChat.Orchestrator` не содержит бизнес-логики мессенджера. Это **инфраструктурный репозиторий**:

- `docker-compose.yml` (+ overlay для dev/prod);
- примеры `.env` (секреты, connection strings, публичные URL);
- скрипты сборки/деплоя;
- системная документация (архитектура, карта репозиториев).

## Порядок старта (целевой)

1. Инфраструктура: PostgreSQL, (опционально) object storage, Seq.
2. `MigrationService` — применение схем БД (one-shot, exit 0).
3. Backend: Identity → Chat → Realtime → Notification / Media / Call (по готовности).
4. `Gateway` — единая точка входа.
5. UI: `Web`, `Admin` (nginx / static).

## Что сервис НЕ делает

- не хранит сообщения и пользователей;
- не шифрует и не расшифровывает контент;
- не выдаёт JWT и не проксирует API сам по себе (это Gateway).

## Зависимости

Все соседние репозитории LocalChat должны лежать рядом на диске (см. README), чтобы compose мог делать `build: context: ../LocalChat.*`.
