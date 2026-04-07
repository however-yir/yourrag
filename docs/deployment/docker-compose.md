# Docker Compose Deployment

## 1. Select profiles

`DOC_ENGINE` controls retrieval backend:

- `elasticsearch`
- `infinity`
- `opensearch`
- `oceanbase`
- `seekdb`

`DEVICE` controls runtime:

- `cpu`
- `gpu`

Example:

```bash
DOC_ENGINE=elasticsearch
DEVICE=cpu
COMPOSE_PROFILES=${DOC_ENGINE},${DEVICE}
COMPOSE_PROJECT_NAME=yourrag
```

## 2. Build and run

```bash
cd docker
docker compose -f docker-compose.yml pull
docker compose -f docker-compose.yml up -d
```

## 3. Scaling examples

Scale task executors:

```bash
docker compose -f docker-compose.yml up -d --scale yourrag-cpu=2
```

## 4. Upgrade strategy

1. Pin image tags in `.env`
2. Backup MySQL, object storage, and Redis
3. Pull new image and perform rolling restart
4. Run smoke tests (`/v1/system/ping`, login, dataset query)

## 5. Rollback strategy

1. Keep previous image tag and compose file snapshot
2. Restore DB snapshot if schema changed
3. Restart with previous image tag
