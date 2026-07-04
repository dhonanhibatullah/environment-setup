include .env

.PHONY: prepare start stop logs

prepare:
	@sudo mkdir -p $(DBEAVER_MOUNT_PATH)
	@sudo chown 1000:1000 $(DBEAVER_MOUNT_PATH)

start: prepare
	@sudo docker compose --env-file .env up -d

stop:
	@sudo docker compose --env-file .env down

logs:
	@sudo docker compose --env-file .env logs -f
