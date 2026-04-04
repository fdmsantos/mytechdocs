VENV := .venvmete
PYTHON := $(VENV)/bin/python
PIP := $(VENV)/bin/pip
MKDOCS := $(VENV)/bin/mkdocs

# Python (local)
$(VENV):
	python3 -m venv $(VENV)
	$(PIP) install -r requirements.txt

serve: $(VENV)
	$(MKDOCS) serve

build: $(VENV)
	$(MKDOCS) build

deploy: $(VENV)
	$(MKDOCS) gh-deploy

# Docker
docker-build:
	docker build -t mytechdocs .

docker-serve:
	docker run --rm -it -p 8000:8000 -v ${PWD}:/docs mytechdocs

docker-run: docker-build docker-serve

.PHONY: serve build deploy docker-build docker-serve docker-run