DEFAULT_GOAL := help

.PHONY: help
help:
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

.PHONY: clean
clean: clean_pyc

.PHONY: clean_pyc
clean_pyc: ## Clean all *.pyc in the system
	find . -type f -name "*.pyc" -delete || true

.PHONY: migrate
migrate: # Runs the migrations
	python $(PROJECT_NAME)/manage.py migrate --noinput

.PHONY: migrations
migrations: ## Generate migrations
	python $(PROJECT_NAME)/manage.py makemigrations

.PHONY: requirements
requirements: ## Install the production requirements
	pip install -r requirements/common.txt

.PHONY: requirements-dev
requirements-dev: ## Install the production requirements
	pip install -r requirements/development.txt

.PHONY: requirements-deployment
requirements-deployment: ## Install the production requirements
	pip install -r requirements/deployment.txt

.PHONY: reusedb_unittests
reusedb_unittests: ## Run all unittests by reusing a db
	cd $(PROJECT_NAME) &&\
	DJANGO_SETTINGS_MODULE=$(PROJECT_NAME).testing.settings REUSE_DB=1 DJANGOENV=testing python manage.py test $(TESTONLY)  --exe --with-specplugin  --keepdb &&\
	cd ..

.PHONY: unittests
unittests: ## Run all unittests by creating a new db
	cd $(PROJECT_NAME) &&\
	DJANGO_SETTINGS_MODULE=$(PROJECT_NAME).testing.settings DJANGOENV=testing python manage.py test $(TESTONLY) --exe --with-specplugin
	cd ..

.PHONY: run
run:
	python $(PROJECT_NAME)/manage.py runserver_plus 0.0.0.0:8000 --settings=$(PROJECT_NAME).$(ENVIRONMENT).settings

.PHONY: shell
shell:
	python $(PROJECT_NAME)/manage.py shell_plus --settings=$(PROJECT_NAME).$(ENVIRONMENT).settings

.PHONY: dramatiq
dramatiq: ## Starts DramatiQ task manager
	cd $(PROJECT_NAME) &&\
	DJANGO_SETTINGS_MODULE=$(PROJECT_NAME).$(ENVIRONMENT).settings python3 manage.py rundramatiq

.PHONY: show_urls
show_urls:
	python $(PROJECT_NAME)/manage.py show_urls --settings=$(PROJECT_NAME).$(ENVIRONMENT).settings

.PHONY: validate_templates
validate_templates:
	python $(PROJECT_NAME)/manage.py validate_templates --settings=$(PROJECT_NAME).$(ENVIRONMENT).settings

.PHONY: createsuperuser
createsuperuser: ## Creates a super user in the database
	DJANGO_SETTINGS_MODULE=$(PROJECT_NAME).$(ENVIRONMENT).settings python3 $(PROJECT_NAME)/manage.py createsuperuser

.PHONY: start_docker
start_docker: ## Starts the dev environment
	docker-compose up -d

.PHONY: start_docker_logs
start_docker_logs: ## Starts the dev environment
	docker-compose up

ifndef VERBOSE
.SILENT:
endif
