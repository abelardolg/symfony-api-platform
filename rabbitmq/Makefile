#!/bin/zsh

RABBIT = symfony-api-platform-rabbitmq
UID	=	$(shell id -u)

help: ## Show this help message
	@echo	'usage:	make	[target]'
	@echo
	@echo	'targets:'
	@egrep	'^(.+)\:\ ##\ (.+)' ${MAKEFILE_LIST} | column -t -c 2 -s ':#'

run: ## Start the containers
	U_ID=${UID} docker-compose up -d

stop:
	U_ID=${UID} docker-compose stop

restart:
	$(MAKE) stop && $(MAKE) run

build:
	U_ID=${UID} docker-compose build

ssh:
	U_ID=${U_ID} docker exec --user ${UID} -it ${RABBIT} bash
