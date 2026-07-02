include .env.production

.PHONY: start stop db.create db.drop

start:
	@sudo docker compose --env-file .env.production -f compose.yml up -d

stop:
	@sudo docker compose --env-file .env.production -f compose.yml down

db.create:
	@if [ -z "$(n)" ]; then echo "Error: database name parameter required (use n=database_name)" && exit 1; fi
	@if [ -z "$(u)" ]; then echo "Error: database user parameter required (use u=username)" && exit 1; fi
	@sudo docker exec $(POSTGRES_CONTAINER_NAME) createdb -U $(u) $(n)

db.drop:
	@if [ -z "$(n)" ]; then echo "Error: database name parameter required (use n=database_name)" && exit 1; fi
	@if [ -z "$(u)" ]; then echo "Error: database user parameter required (use u=username)" && exit 1; fi
	@sudo docker exec $(POSTGRES_CONTAINER_NAME) dropdb -U $(u) $(n)
