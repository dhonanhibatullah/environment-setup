include .env.production

.PHONY: prepare start stop logs db.create db.drop

prepare:
	@sudo mkdir -p $(POSTGRES_MOUNT_PATH)
	@sudo chown 1000:1000 $(POSTGRES_MOUNT_PATH)

start: prepare
	@sudo docker compose --env-file .env.production -f compose.yml up -d

stop:
	@sudo docker compose --env-file .env.production -f compose.yml down

logs:
	@sudo docker compose --env-file .env.production -f compose.yml logs -f

db.create:
	@if [ -z "$(n)" ]; then echo "Error: database name parameter required (use n=database_name)" && exit 1; fi
	@if [ -z "$(u)" ]; then echo "Error: database user parameter required (use u=username)" && exit 1; fi
	@sudo docker exec $(POSTGRES_CONTAINER_NAME) createdb -U $(u) $(n)

db.drop:
	@if [ -z "$(n)" ]; then echo "Error: database name parameter required (use n=database_name)" && exit 1; fi
	@if [ -z "$(u)" ]; then echo "Error: database user parameter required (use u=username)" && exit 1; fi
	@sudo docker exec $(POSTGRES_CONTAINER_NAME) dropdb -U $(u) $(n)
