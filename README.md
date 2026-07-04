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

Branch-neutral services use the same `.env` filename everywhere:

```bash
cd dbeaver
cp .env.example .env
nano .env
make start
```

```bash
cd desktop
cp .env.example .env
nano .env
make build
make start
```

Stop a service with:

```bash
make stop
```

Follow logs with:

```bash
make logs
```

Local `.env*` files and `data` directories are ignored by Git. Persistent storage is controlled by each service's `*_MOUNT_PATH` value, or `DESKTOP_USER_MOUNT_PATH` for the desktop home mount, with `./data` as the Compose fallback where configured.

## Services

| Service | Directory | Container | Host ports | Notes |
| --- | --- | --- | --- | --- |
| PostgreSQL / TimescaleDB | `postgres` | `development-postgres` | `15432 -> 5432` | Default database/user/password are defined in `postgres/.env.example`. |
| Redis | `redis` | `development-redis` | `16379 -> 6379` | Requires `REDIS_PASSWORD`. |
| EMQX | `emqx` | `development-emqx` | `11883 -> 1883`, `18883 -> 8883`, `18084 -> 8083`, `18085 -> 8084`, `18083 -> 18083` | Dashboard user is `admin`; password is `EMQX_DASHBOARD_PASSWORD`. MQTT users are managed in the EMQX Dashboard. |
| MinIO | `minio` | `development-minio` | `19000 -> 9000`, `19001 -> 9001` | Root credentials are defined by `MINIO_ROOT_USER` and `MINIO_ROOT_PASSWORD`. |
| DBeaver / CloudBeaver | `dbeaver` | `dbeaver` | `8978 -> 8978` | Shared database management UI across branches. Complete first-launch setup in the browser. |
| Kasm Ubuntu Desktop | `desktop` | `desktop` | `6901 -> 6901` | Shared browser-accessible Ubuntu Noble desktop with ROS 2 Jazzy, Gazebo, RViz, CUDA Toolkit 12.x, NVIDIA GPU access, native AI build tools, and passwordless sudo for `kasm-user`. Open `https://localhost:6901` and login with `kasm_user` plus `DESKTOP_VNC_PASSWORD`. |

## Desktop GPU Access

The `desktop` service requests all NVIDIA GPUs through Docker Compose. The host must already have the NVIDIA driver and NVIDIA Container Toolkit installed. A GTX 1050 Ti with `nvidia-driver-535` is fine as long as these commands work on the host:

```bash
nvidia-smi
docker run --rm --gpus all ubuntu nvidia-smi
```

Inside the desktop, verify the robotics and GPU tooling with:

```bash
sudo -n true
ros2 --version
rviz2
gz sim
glxinfo -B
nvidia-smi
nvcc --version
```

The Kasm user home persists at `DESKTOP_USER_MOUNT_PATH` and is mounted to `/home/kasm-user`. The ROS 2 workspace lives inside that mount at `/home/kasm-user/ros2_ws`.

CUDA Toolkit 12.x is installed for compiling CUDA code, custom AI operators, and native GPU projects. Python AI libraries are not installed globally. Create a project virtual environment inside the mounted workspace instead:

```bash
cd ~/ros2_ws
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
```

## PostgreSQL Helpers

The Postgres Makefile includes database helper targets:

```bash
make db.create n=<database_name> u=<username>
make db.drop n=<database_name> u=<username>
```

These run `createdb` and `dropdb` inside the container named by `POSTGRES_CONTAINER_NAME`.
