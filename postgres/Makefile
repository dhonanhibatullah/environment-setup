include .env

.PHONY: netcheck install stop uninstall db_create db_drop

netcheck:
	@sudo docker network inspect donspace-net >/dev/null 2>&1 || sudo docker network create donspace-net

volumecheck:
	@if [ ! -f .env ]; then echo "Error: .env file not found" && exit 1; fi
	@echo "Checking volumes..."
	@sudo mkdir -p $(POSTGRES_MOUNT_PATH)
	@sudo chown -R 1000:1000 $(POSTGRES_MOUNT_PATH)
	@sudo chmod 700 $(POSTGRES_MOUNT_PATH)

install: netcheck volumecheck
	@sudo docker compose up -d

stop:
	@sudo docker compose down

uninstall:
	@sudo docker compose down -v

db_create:
	@if [ -z "$(n)" ]; then echo "Error: database name parameter required (use n=database_name)" && exit 1; fi
	@if [ -z "$(u)" ]; then echo "Error: database user parameter required (use u=username)" && exit 1; fi
	@sudo docker exec donspace-postgres createdb -U $(u) $(n)

db_drop:
	@if [ -z "$(n)" ]; then echo "Error: database name parameter required (use n=database_name)" && exit 1; fi
	@if [ -z "$(u)" ]; then echo "Error: database user parameter required (use u=username)" && exit 1; fi
	@sudo docker exec donspace-postgres dropdb -f -U $(u) $(n)
