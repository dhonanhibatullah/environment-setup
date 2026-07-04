include .env.development

.PHONY: prepare start stop logs

prepare:
	@sudo mkdir -p $(EMQX_MOUNT_PATH)/data $(EMQX_MOUNT_PATH)/log
	@sudo chown 1000:1000 $(EMQX_MOUNT_PATH) $(EMQX_MOUNT_PATH)/data $(EMQX_MOUNT_PATH)/log

start: prepare
	@sudo docker compose --env-file .env.development up -d

stop:
	@sudo docker compose --env-file .env.development down

logs:
	@sudo docker compose --env-file .env.development logs -f
