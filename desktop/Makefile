include .env

.PHONY: start stop logs

start:
	@sudo docker compose --env-file .env up -d

stop:
	@sudo docker compose --env-file .env down

logs:
	@sudo docker compose --env-file .env logs -f
