# Docker Compose Setup

## Build and start

```bash
docker compose up -d --build
```

## Check status

```bash
docker compose ps
```

## Open UI

```text
http://localhost:5173
```

Demo login:

```text
admin@devopsshack.com
admin123
```

## Verify every service through the frontend reverse proxy

```bash
curl http://localhost:5173/api/auth/health
curl http://localhost:5173/api/catalog/health
curl http://localhost:5173/api/inventory/health
curl http://localhost:5173/api/orders/health
curl http://localhost:5173/api/payments/health
curl http://localhost:5173/api/notifications/health
curl http://localhost:5173/api/analytics/health
```

## Logs

```bash
docker compose logs -f
```

Single service:

```bash
docker compose logs -f order-service
```

## Database verification

```bash
docker compose exec postgres psql -U postgres -c "\\l"
```

## Stop

```bash
docker compose down
```

## Stop and delete PostgreSQL data

```bash
docker compose down -v
```

Use `-v` only when you intentionally want a fresh database initialization.
