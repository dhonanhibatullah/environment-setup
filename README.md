# Environment Setup

Local Docker Compose setup for development services. Each service is managed from its own directory with a service-local `.env`, `compose.yml`, and `Makefile`.

## Prerequisites

- Docker Engine: https://docs.docker.com/engine/
- Make: https://www.gnu.org/software/make/

## Usage

Environment-specific services follow the branch env file pattern:

```bash
cd <service>
cp .env.example .env.development
nano .env.development
make start
```

DBeaver is shared across branches, so it uses the same `.env` filename everywhere:

```bash
cd dbeaver
cp .env.example .env
nano .env
make start
```

Stop a service with:

```bash
make stop
```

Local `.env*` files and `data` directories are ignored by Git. Persistent storage is controlled by each service's `*_MOUNT_PATH` value, with `./data` as the Compose fallback where configured.

## Services

| Service | Directory | Container | Host ports | Notes |
| --- | --- | --- | --- | --- |
| PostgreSQL / TimescaleDB | `postgres` | `development-postgres` | `15432 -> 5432` | Default database/user/password are defined in `postgres/.env.example`. |
| Redis | `redis` | `development-redis` | `16379 -> 6379` | Requires `REDIS_PASSWORD`. |
| EMQX | `emqx` | `development-emqx` | `11883 -> 1883`, `18883 -> 8883`, `18084 -> 8083`, `18085 -> 8084`, `18083 -> 18083` | Dashboard user is `admin`; password is `EMQX_DASHBOARD_PASSWORD`. MQTT users are managed in the EMQX Dashboard. |
| MinIO | `minio` | `development-minio` | `19000 -> 9000`, `19001 -> 9001` | Root credentials are defined by `MINIO_ROOT_USER` and `MINIO_ROOT_PASSWORD`. |
| DBeaver / CloudBeaver | `dbeaver` | `dbeaver` | `18978 -> 8978` | Shared database management UI across branches. Complete first-launch setup in the browser. |

## PostgreSQL Helpers

The Postgres Makefile includes database helper targets:

```bash
make db.create n=<database_name> u=<username>
make db.drop n=<database_name> u=<username>
```

These run `createdb` and `dropdb` inside the container named by `POSTGRES_CONTAINER_NAME`.
