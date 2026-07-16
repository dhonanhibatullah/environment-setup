include .env.development

.PHONY: prepare start stop logs

prepare:
	@sudo mkdir -p $(KEYCLOAK_MOUNT_PATH)
	@sudo chown 1000:1000 $(KEYCLOAK_MOUNT_PATH)

start: prepare
	@sudo docker compose --env-file .env.development up -d

stop:
	@sudo docker compose --env-file .env.development down

logs:
	@sudo docker compose --env-file .env.development logs -f
