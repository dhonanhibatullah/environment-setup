include .env

.PHONY: start stop

start:
	@sudo docker compose --env-file .env up -d

stop:
	@sudo docker compose --env-file .env down
