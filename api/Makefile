#!/bin/zsh

DOCKER_BE = symfony-api-platform-be
OS	:=	${shell uname}

ifeq ($(OS),Darwin)
	UID	=	$(shell id -u)
else ifeq ($(OS),Linux)
	UID	=	$(shell id -u)
else
	UID	=	1000
endif

help: ## Show this help message
	@echo	'usage:	make	[target]'
	@echo
	@echo	'targets:'
	@egrep	'^(.+)\:\ ##\ (.+)' ${MAKEFILE_LIST} | column -t -c 2 -s ':#'

run: ## Start the containers
	docker network create symfony-api-platform-network || true
	U_ID=${UID} docker-compose up -d

stop:
	U_ID=${UID} docker-compose stop

restart:
	$(MAKE) stop && $(MAKE) run

build:
	U_ID=${UID} docker-compose build

prepare:
	$(MAKE) composer-install

composer-install:
	U_ID=${U_ID} docker exec --user ${UID} -it ${DOCKER_BE} composer install --no-scripts --no-interaction --optimize-autoloader

migrations:
	U_ID=${U_ID} docker exec --user ${UID} -it ${DOCKER_BE} bin/console doctrine:migrations:migrate -n

be-logs:
	U_ID=${U_ID} docker exec --user ${UID} -it ${DOCKER_BE} tail -f var/log/dev.log

ssh-be:
	U_ID=${U_ID} docker exec --user ${UID} -it ${DOCKER_BE} bash

code-style:
	U_ID=${U_ID} docker exec --user ${UID} -it ${DOCKER_BE} php-cs-fixer fix src --rules=@Symfony
