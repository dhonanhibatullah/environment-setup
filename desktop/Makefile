include .env

.PHONY: prepare build start stop logs

prepare:
	@sudo mkdir -p $(DESKTOP_USER_MOUNT_PATH)/ros2_ws/src
	@sudo chown 1000:1000 $(DESKTOP_USER_MOUNT_PATH) $(DESKTOP_USER_MOUNT_PATH)/ros2_ws $(DESKTOP_USER_MOUNT_PATH)/ros2_ws/src

build:
	@sudo docker compose --env-file .env build

start: prepare
	@sudo docker compose --env-file .env up -d

stop:
	@sudo docker compose --env-file .env down

logs:
	@sudo docker compose --env-file .env logs -f
