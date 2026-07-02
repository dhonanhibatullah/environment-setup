include .env.production

.PHONY: start stop

start:
	@sudo docker compose --env-file .env.production up -d

stop:
	@sudo docker compose --env-file .env.production down
