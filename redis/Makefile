include .env.development

.PHONY: start stop

start: 
	@sudo docker compose --env-file .env.development up -d

stop:
	@sudo docker compose --env-file .env.development down
