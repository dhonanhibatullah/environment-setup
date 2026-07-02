include .env.staging

.PHONY: start stop

start:
	@sudo docker compose --env-file .env.staging up -d

stop:
	@sudo docker compose --env-file .env.staging down
