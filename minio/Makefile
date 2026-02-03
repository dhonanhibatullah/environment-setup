include .env

.PHONY: netcheck install stop uninstall

netcheck:
	@sudo docker network inspect donspace-net >/dev/null 2>&1 || sudo docker network create donspace-net

volumecheck:
	@if [ ! -f .env ]; then echo "Error: .env file not found" && exit 1; fi
	@echo "Checking volumes..."
	@sudo mkdir -p $(MINIO_MOUNT_PATH)
	@sudo chown -R 0:0 $(MINIO_MOUNT_PATH)
	@sudo chmod 755 $(MINIO_MOUNT_PATH)

install: netcheck volumecheck
	@sudo docker compose up -d

stop:
	@sudo docker compose down

uninstall:
	@sudo docker compose down -v
