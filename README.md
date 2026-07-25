# LocalChat.Orchestrator

Точка входа для локального и серверного запуска стека **LocalChat**: Docker Compose, окружение, скрипты деплоя и общая документация архитектуры.

## Назначение

- собрать все сервисы в единый compose-стек;
- описать порядок старта (миграции → backend → realtime → gateway → UI);
- хранить `.env` примеры и общую карту портов/зависимостей.

## Стек

- Docker Compose v2
- PostgreSQL (общая инфраструктура или отдельные БД по сервисам)
- Seq / структурированные логи (по мере внедрения)

## Документация

- [Обзор сервиса](docs/overview.md)
- [Архитектура системы](docs/architecture.md)
- [Карта репозиториев](docs/repositories.md)
- [TODO до полной реализации](docs/todo.md)

## Локальная раскладка репозиториев

Ожидается соседство клонов:

```
local_chat/
  LocalChat.Orchestrator/   ← вы здесь
  LocalChat.Gateway/
  LocalChat.IdentityService/
  LocalChat.ChatService/
  LocalChat.RealtimeService/
  LocalChat.NotificationService/
  LocalChat.MediaService/
  LocalChat.CallService/
  LocalChat.MigrationService/
  LocalChat.Web/
  LocalChat.Admin/
```

## Запуск

Compose-файлы и скрипты появятся по мере реализации MVP. Пока см. [docs/todo.md](docs/todo.md).

## Связанные репозитории

| Репозиторий | Роль |
|---|---|
| [LocalChat.Gateway](https://github.com/MuskatGroup/LocalChat.Gateway) | Единый HTTP/WS вход |
| [LocalChat.IdentityService](https://github.com/MuskatGroup/LocalChat.IdentityService) | Пользователи, заявки, JWT |
| [LocalChat.ChatService](https://github.com/MuskatGroup/LocalChat.ChatService) | Диалоги и ciphertext |
| [LocalChat.RealtimeService](https://github.com/MuskatGroup/LocalChat.RealtimeService) | SignalR: доставка, presence |
| [LocalChat.NotificationService](https://github.com/MuskatGroup/LocalChat.NotificationService) | Push / email |
| [LocalChat.MediaService](https://github.com/MuskatGroup/LocalChat.MediaService) | Фото/видео (encrypted blobs) |
| [LocalChat.CallService](https://github.com/MuskatGroup/LocalChat.CallService) | Signaling звонков |
| [LocalChat.MigrationService](https://github.com/MuskatGroup/LocalChat.MigrationService) | One-shot EF migrations |
| [LocalChat.Web](https://github.com/MuskatGroup/LocalChat.Web) | Angular-клиент мессенджера |
| [LocalChat.Admin](https://github.com/MuskatGroup/LocalChat.Admin) | Angular-админка заявок |

## Лицензия

Apache-2.0 — см. [LICENSE](LICENSE).
