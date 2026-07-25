# Карта репозиториев LocalChat

Организация: [MuskatGroup](https://github.com/orgs/MuskatGroup/repositories).

| Репозиторий | Тип | Порт (план) | Роль |
|---|---|---|---|
| [LocalChat.Orchestrator](https://github.com/MuskatGroup/LocalChat.Orchestrator) | Infra | — | Compose, env, общая docs |
| [LocalChat.Gateway](https://github.com/MuskatGroup/LocalChat.Gateway) | .NET | 5000 | YARP / единый вход HTTP+WS |
| [LocalChat.IdentityService](https://github.com/MuskatGroup/LocalChat.IdentityService) | .NET | 5101 | Заявки, пользователи, JWT, device keys |
| [LocalChat.ChatService](https://github.com/MuskatGroup/LocalChat.ChatService) | .NET | 5102 | Диалоги, membership, ciphertext |
| [LocalChat.RealtimeService](https://github.com/MuskatGroup/LocalChat.RealtimeService) | .NET | 5103 | SignalR: delivery, typing, presence |
| [LocalChat.NotificationService](https://github.com/MuskatGroup/LocalChat.NotificationService) | .NET | 5104 | Push / email (P1) |
| [LocalChat.MediaService](https://github.com/MuskatGroup/LocalChat.MediaService) | .NET | 5105 | Encrypted blobs фото/видео (P1) |
| [LocalChat.CallService](https://github.com/MuskatGroup/LocalChat.CallService) | .NET | 5106 | WebRTC signaling (P2) |
| [LocalChat.MigrationService](https://github.com/MuskatGroup/LocalChat.MigrationService) | .NET | — | One-shot EF migrations |
| [LocalChat.Web](https://github.com/MuskatGroup/LocalChat.Web) | Angular | 3000 | Клиент мессенджера + E2EE |
| [LocalChat.Admin](https://github.com/MuskatGroup/LocalChat.Admin) | Angular | 3001 | Подтверждение заявок |

Порты ориентировочные; финальные значения фиксируются в `.env` Orchestrator.

## Клиенты

- **Web** и **Admin** — Angular (единый фронтенд-стек).
- Оба ходят только в **Gateway**, не напрямую в микросервисы (кроме явных исключений для отладки).

## Базы данных (план)

| Сервис | БД / схема |
|---|---|
| IdentityService | `localchat_identity` |
| ChatService | `localchat_chat` |
| MediaService | `localchat_media` (+ object storage) |
| CallService | `localchat_call` (сессии/участники) |
| NotificationService | `localchat_notification` (опционально) |

Realtime может быть stateless (connections в памяти / Redis позже).
