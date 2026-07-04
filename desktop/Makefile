include .env

.PHONY: build start stop logs

build:
	@sudo docker compose --env-file .env build

start:
	@sudo docker compose --env-file .env up -d

stop:
	@sudo docker compose --env-file .env down

logs:
	@sudo docker compose --env-file .env logs -f
