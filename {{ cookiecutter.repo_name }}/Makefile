.ONESHELL:
.PHONY: install create_environment activate data clean lint format check-format sync_data_to_s3 sync_data_from_s3

SHELL = /bin/bash
PROJECT_DIR := $(shell dirname $(realpath $(lastword $(MAKEFILE_LIST))))
ROOT_DIR := $(abspath $(abspath $(lastword $(MAKEFILE_LIST))/../../)/../)
BUCKET = output
PROFILE = default
PROJECT_NAME = {{ cookiecutter.repo_name }}
PYTHON_INTERPRETER = python3


ifeq (,$(shell which conda))
HAS_CONDA=False
else
HAS_CONDA=True
CONDA_ACTIVATE=source $$(conda info --base)/etc/profile.d/conda.sh ; conda activate ; conda activate
endif

install:
	@echo "Installing..."
{% if cookiecutter.package_manager == "conda" %}
ifeq (True,$(HAS_CONDA))
	@echo ">>> Detected conda, creating conda environment."
ifeq (3,$(findstring 3,$(PYTHON_INTERPRETER)))
	tar -xf "$(ROOT_DIR)/setup/albatross_packages/preprocessing_data_models.tar.gz" -C /tmp
	conda env create --quiet -f environment.yml
	conda init bash
	@$(CONDA_ACTIVATE) $(PROJECT_NAME)
	$(PYTHON_INTERPRETER) -m pip install --quiet poetry
	poetry install
	poetry add "$(ROOT_DIR)/setup/config_bml/." /tmp/wheel/al_ai_pre_processing_data_models-*-py3-none-any.whl
	poetry run pre-commit install
endif
else
	@echo ">>> Add conda to your shell and run make create_environment again"
endif
{% elif cookiecutter.package_manager == "poetry" %}
	tar -xf "$(ROOT_DIR)/setup/albatross_packages/preprocessing_data_models.tar.gz" -C /tmp
	pipx install poetry 
	$(PYTHON_INTERPRETER) -m venv env
	poetry env use env/bin/python3
	poetry install
	poetry add "$(ROOT_DIR)/setup/config_bml/." /tmp/wheel/al_ai_pre_processing_data_models-*-py3-none-any.whl 
	poetry run pre-commit install
{% endif %}


data:
	$(PYTHON_INTERPRETER) src/make_dataset.py

train:
	$(PYTHON_INTERPRETER) src/make_training.py

clean:
	find . -type f -name "*.py[co]" -delete
	find . -type d -name "__pycache__" -delete
	rm -rf notebooks/.ipynb_checkpoints

format:
	black --config pyproject.toml .

check-format:
	black --config pyproject.toml --check .

check:
	mypy --config-file pyproject.toml .
	flake8 --toml-config pyproject.toml .

docs_view:
	@echo View API documentation...
	pdoc src --http localhost:8080

docs_save:
	@echo Save documentation to docs...
	pdoc src -o docs
