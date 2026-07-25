# Архитектура LocalChat

## Цель продукта

Закрытый мессенджер с:

- заявкой на доступ и подтверждением в админке;
- end-to-end шифрованием сообщений на клиенте;
- заделом под групповые чаты, медиа (фото/видео) и звонки (1:1 и групповые).

## Ключевые правила

1. **Сервер — relay + метаданные.** Plaintext и приватные ключи живут только на клиенте.
2. **E2EE:** Signal-like подход — X25519 (+ позже PQ hybrid), HKDF, ChaCha20-Poly1305 / AES-GCM, ratchet.
3. **Conversation** сразу моделируется как `Direct | Group` (UI групп — позже).
4. **Медиа** не хранятся в Chat: сообщение содержит `mediaRefs[]`.
5. **Звонки** — отдельный CallService (signaling; SFU позже), не смешиваются с доменной моделью текста.
6. **Админка** — отдельное Angular-приложение и admin-контур прав в Identity.

## Компоненты

```mermaid
flowchart LR
  Web[LocalChat.Web_Angular]
  Admin[LocalChat.Admin_Angular]
  Gw[LocalChat.Gateway]
  Id[IdentityService]
  Chat[ChatService]
  Rt[RealtimeService]
  Media[MediaService]
  Call[CallService]
  Notif[NotificationService]

  Web --> Gw
  Admin --> Gw
  Gw --> Id
  Gw --> Chat
  Gw --> Rt
  Gw --> Media
  Gw --> Call
  Gw --> Notif
  Chat -.->|events| Rt
  Id -.->|accessApproved| Notif
  Call -.->|signaling| Rt
```

## Поток: заявка на доступ

```mermaid
sequenceDiagram
  participant User as Web
  participant Gw as Gateway
  participant Id as IdentityService
  participant Adm as Admin
  participant N as NotificationService

  User->>Gw: POST access-request
  Gw->>Id: создать заявку Pending
  Id-->>User: статус Pending
  Adm->>Gw: GET pending requests
  Gw->>Id: список заявок
  Adm->>Gw: POST approve/reject
  Gw->>Id: смена статуса
  Id-->>N: событие AccessApproved
  N-->>User: уведомление (позже)
```

Только пользователи со статусом `Approved` получают рабочий JWT и доступ к чату.

## Поток: сообщение 1:1 (E2EE)

```mermaid
sequenceDiagram
  participant A as Web_A
  participant Gw as Gateway
  participant Chat as ChatService
  participant Rt as RealtimeService
  participant B as Web_B

  A->>A: encrypt plaintext AEAD
  A->>Gw: POST message ciphertext
  Gw->>Chat: сохранить blob + header
  Chat-->>Rt: событие NewMessage
  Rt-->>B: push ciphertext
  B->>B: decrypt
```

Сервер видит ciphertext, nonce, публичные ratchet/header поля, `conversationId`, `senderDeviceId` — не текст.

## Будущее: медиа

1. Клиент шифрует файл локально.
2. `MediaService` принимает encrypted blob, возвращает `mediaId`.
3. `ChatService` сохраняет сообщение с `mediaRefs: [mediaId]`.
4. Получатель скачивает blob и расшифровывает клиентским ключом.

## Будущее: звонки

1. `CallService` создаёт сессию и обменивает SDP/ICE (signaling).
2. Медиапоток — WebRTC peer-to-peer (1:1) или через SFU (группы).
3. События «звонок начат/завершён» могут отражаться в Chat как системные ciphertext/events, без RTP в ChatService.

## Приоритеты реализации

| Фаза | Состав |
|---|---|
| **P0 MVP** | Orchestrator, Gateway, Identity, Chat, Realtime, Migration, Web, Admin |
| **P1** | Media, Notification |
| **P2** | Call, UI групп, SFU |

Подробные чеклисты — в `docs/todo.md` каждого сервиса.
