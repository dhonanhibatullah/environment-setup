include .env.production

.PHONY: prepare start stop logs

prepare:
	@sudo mkdir -p $(REDIS_MOUNT_PATH)
	@sudo chown 1000:1000 $(REDIS_MOUNT_PATH)

start: prepare
	@sudo docker compose --env-file .env.production up -d

stop:
	@sudo docker compose --env-file .env.production down

logs:
	@sudo docker compose --env-file .env.production logs -f
