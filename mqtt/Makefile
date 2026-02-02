include .env

.PHONY: netcheck pwdgen clean install stop uninstall

netcheck:
	@sudo docker network inspect donspace-net >/dev/null 2>&1 || sudo docker network create donspace-net

pwdgen:
	@sudo rm -f password.txt
	@touch password.txt
	@sudo chown 1883:1883 password.txt
	@sudo chmod 0700 password.txt
	@sudo docker run --rm --user 1883:1883 -v $(CURDIR)/password.txt:/tmp/password.txt eclipse-mosquitto mosquitto_passwd -b /tmp/password.txt $(MQTT_USERNAME) $(MQTT_PASSWORD)
	@echo "Password file generated"

clean:
	@sudo rm -f password.txt

install: netcheck pwdgen
	@if [ ! -f .env ]; then echo "Error: .env file not found" && exit 1; fi
	@sudo docker compose up -d

stop:
	@sudo docker compose down

uninstall: clean
	@sudo docker compose down -v

