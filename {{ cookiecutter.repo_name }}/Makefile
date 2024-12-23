.ONESHELL:
.PHONY: install create_environment activate data clean lint format check-format sync_data_to_s3 sync_data_from_s3

SHELL = /bin/bash
PROJECT_DIR := $(shell dirname $(realpath $(lastword $(MAKEFILE_LIST))))
BUCKET = output
PROFILE = default
PROJECT_NAME = {{ cookiecutter.repo_name }}
PYTHON_INTERPRETER = python3


ifeq (,$(shell which mamba))
HAS_MAMBA=False
else
HAS_MAMBA=True
CONDA_ACTIVATE=source $$(conda info --base)/etc/profile.d/conda.sh ; conda activate ; conda activate
endif

ifeq (,$(shell which poetry))
HAS_POETRY=False
else
HAS_POETRY=True
endif

install:
	@echo "Installing..."
{% if cookiecutter.package_manager == "mamba" %}
ifeq (True,$(HAS_MAMBA))
	@echo ">>> Detected mamba, creating conda environment"
ifeq (3,$(findstring 3,$(PYTHON_INTERPRETER)))
	mamba env create -f environment.yml
	conda init bash
	@$(CONDA_ACTIVATE) $(PROJECT_NAME)
	$(PYTHON_INTERPRETER) -m pip install --quiet poetry
	poetry install
	poetry run pre-commit install
endif
else
	@echo ">>> Add mamba to your shell and run make install again"
endif
{% elif cookiecutter.package_manager == "poetry" %}
ifeq (True,$(HAS_POETRY))
	@echo ">>> Detected poetry, creating poetry environment"
ifeq (3,$(findstring 3,$(PYTHON_INTERPRETER)))
	poetry config virtualenvs.in-project true
	poetry install
	poetry run pre-commit install
endif
else 
	@echo ">>> Add poetry to your shell and run make install again"
endif
{% endif %}

data:
	$(PYTHON_INTERPRETER) src/make_dataset.py

train:
	$(PYTHON_INTERPRETER) src/train_model.py

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
	pdoc3 src --http localhost:6006

docs_save:
	@echo Save documentation to docs...
	pdoc3 --html src
