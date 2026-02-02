include .env

.PHONY: netcheck install stop uninstall

netcheck:
	@sudo docker network inspect donspace-net >/dev/null 2>&1 || sudo docker network create donspace-net

install: netcheck
	@if [ ! -f .env ]; then echo "Error: .env file not found" && exit 1; fi
	@sudo docker compose up -d

stop:
	@sudo docker compose down

uninstall:
	@sudo docker compose down -v
