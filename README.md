# Cookiecutter Data Science

A modified version of cookiecutter Data Science : 

> _A logical, reasonably standardized, but flexible project structure for doing and sharing data science work._
>
> _[Project homepage](http://drivendata.github.io/cookiecutter-data-science/)_

---

### Requirements

- Python 2.7 or 3.5+
- [Cookiecutter Python package](http://cookiecutter.readthedocs.org/en/latest/installation.html) >= 1.4.0: This can be installed with pip by or conda depending on how you manage your Python packages:

```bash
$ pip install cookiecutter pipx poetry
```

or

```bash
$ pip install pipx poetry
$ conda config --add channels conda-forge
$ conda install cookiecutter
```

### Install

```bash
cookiecutter git@github.com:marionpavaux/cookie_cutter_data_science.git
```

[![asciicast](https://asciinema.org/a/244658.svg)](https://asciinema.org/a/244658)

Follow the instructions in the terminal, then go in the project directory and run:

```bash
make install
```

Install all packages needed for your environment using the name of the project as the environment name. Use mamba (conda-based), or use poetry (pip-based). Poetry will create a poetry.lock file that is equivalent to requirements.txt.

### Directory structure

The directory structure of your new project looks like this:

```
    ├── LICENSE
    ├── Makefile           <- Makefile with commands like `make install` or `make clean`
    ├── README.md          <- The top-level README for developers using this project.
    ├── data
    │   ├── external       <- Data from third party sources.
    │   ├── intermediate   <- Intermediate data that has been transformed.
    |   ├── preprocessed   <- Pre-processed data from data_processing package.
    │   ├── processed      <- The final, canonical data sets for modeling.
    │   └── raw            <- The original, immutable data dump.
    │
    ├── config             <- Configuration files for launching scripts or constants in src folder.
    │
    ├── models             <- Trained and serialized models, model predictions, or model summaries
    │
    ├── notebooks          <-Jupyter notebooks. Naming convention is a number (for ordering).
    │
    ├── scripts            <-Scritps. Naming convention is a number (for ordering).
    │
    ├── references         <- Data dictionaries, manuals, and all other explanatory materials.
    │
    ├── reports            <- Generated analysis as HTML, PDF, LaTeX, etc.
    │   └── figures        <- Generated graphics and figures to be used in reporting
    │
    ├── scripts            <- Scripts.
    │   ├── make_dataset.py           <- Clean and process data to construct training/validation datasets.
    │   ├── train_model.py          <- Load, train and save models. 
    │   └── eval_model.py          <- Load, train and save models. 
    │
    ├── src/{{ cookiecutter.repo_name | replace("-", "_") }}  <- Packagable source code for this project.
    │   ├── __init__.py    <- Makes src a Python module
    │   │
    │   ├── data           <- Package to download or generate data. clean is only an example.
    │   │   ├── __init__.py
    │   │   ├── clean.py
    │   │   └── features.py
    │   │
    │   ├── dl         <- Package to train models and then use trained models to make
    │   │   ├── __init__.py                 predictions. models, predictor and trainer are only examples.
    │   │   ├── dataloaders.py
    │   │   ├── models.py
    │   │   ├── trainer.py
    │   │   └── predictor.py
    │   │
    │   └── visualization  <- Scripts to create exploratory and results oriented visualizations
    │       ├── __init__.py
    │       └── visualize.py
    │ 
    ├── tests              <- Contains tests.
    │ 
    {% if cookiecutter.package_manager == 'mamba' %}├── environment.yml       <- Package requirements.{% endif %} 
    ├── .gitignore
    ├── .pre-commit-config.yaml <- Applies black and nbstripout, and checks pep8 conventions as well as doctring percentage
    ├── poetry.lock        <- Keeps track of poetry dependencies after install
    └── pyproject.toml     <- Specifies environment variables

```
