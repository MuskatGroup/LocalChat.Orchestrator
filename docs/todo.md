# Orchestrator — TODO

## P0 — MVP

- [ ] `docker-compose.yml` с postgres (+ seq по желанию)
- [ ] overlay `docker-compose.dev.yml` / `docker-compose.prod.yml`
- [ ] `.env.example` с портами, JWT secret, connection strings
- [ ] сервисы MVP в compose: migration, identity, chat, realtime, gateway, web, admin
- [ ] healthcheck-зависимости и корректный `depends_on`
- [ ] скрипты `scripts/deploy/deploy_dev.sh` (и Windows-аналог при необходимости)
- [ ] README: команды `up` / `down` / `logs`

## P1

- [ ] подключить MediaService и NotificationService в compose
- [ ] volume / S3-compatible storage для медиа
- [ ] единый `X-Correlation-Id` в логах всех сервисов

## P2

- [ ] CallService + (опционально) SFU-контейнер (LiveKit / mediasoup)
- [ ] staging/prod профили, секреты вне git
- [ ] backup/restore postgres

## Не входит в Orchestrator

- бизнес-API и криптография клиентов — в соответствующих репозиториях.
