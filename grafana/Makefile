include .env

.PHONY: netcheck install stop uninstall

netcheck:
	@sudo docker network inspect donspace-net >/dev/null 2>&1 || sudo docker network create donspace-net

volumecheck:
	@if [ ! -f .env ]; then echo "Error: .env file not found" && exit 1; fi
	@echo "Checking volumes..."
	@sudo mkdir -p $(GRAFANA_MOUNT_PATH)
	@sudo chown -R 472:472 $(GRAFANA_MOUNT_PATH)
	@sudo chmod 755 $(GRAFANA_MOUNT_PATH)
	@sudo mkdir -p $(LOKI_MOUNT_PATH)
	@sudo chown -R 0:0 $(LOKI_MOUNT_PATH)
	@sudo chmod 755 $(LOKI_MOUNT_PATH)

install: netcheck volumecheck
	@sudo docker compose up -d

stop:
	@sudo docker compose down

uninstall:
	@sudo docker compose down -v
