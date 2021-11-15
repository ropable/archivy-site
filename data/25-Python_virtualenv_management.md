---
date: 02-09-21
id: 25
path: ''
tags:
- python
title: Python environment management with pyenv and Poetry
type: bookmark
url: https://ropable.com/2020/python-pyenv-poetry.html
---

This is a summary of the workflow for managing Python virtual environments using modern tools.

Links:

  * pyenv: <https://github.com/pyenv/pyenv>
  * pyenv installer: <https://github.com/pyenv/pyenv-installer>
  * pyenv tutorial: <https://amaral.northwestern.edu/resources/guides/pyenv-tutorial>
  * Poetry: <https://python-poetry.org/>
  * Hypermodern Python setup: <https://cjolowicz.github.io/posts/hypermodern-python-01-setup/>
  * Set up an awesome Python environment: <https://towardsdatascience.com/how-to-setup-an-awesome-python-environment-for-data-science-or-anything-else-35d358cc95d5>
  * Overview of Python dependency management tools: <https://modelpredict.com/python-dependency-management-tools>

# Pyenv

Use **pyenv** to install different versions of Python on a host easily, and enable easy switching between them. To install, use the automatic installer (https://github.com/pyenv/pyenv-installer) or follow the project [installation instructions](https://github.com/pyenv/pyenv#installation). The automatic installer also includes a couple of additional plugins (pyenv-virtualenv, pyenv-update).

**NOTE**: depending on the installation method and your environment, changes might be required in your `.profile` and/or `.bashrc` files.

Thereafter, use pyenv to install different Python versions:

    pyenv install 3.7.7
    pyenv install 3.8.5
    pyenv install 3.9.6

Note that the `install` command has a `--list` switch to display available Python versions to be installed:

    pyenv install --list | grep 3.9
      3.9.0
      3.9-dev
      3.9.1
      3.9.2
      3.9.3
      3.9.4
      miniconda-3.9.1
      miniconda3-3.9.1

Set a default Python version to be used upon changing into a project directory:

    pyenv local 3.7.7

Don't add the `.python-version` file to the repository (this is a local setting); add it to `.gitignore` if necessary. If pyenv is configured properly, the presence of `.python-version` will cause it to activate and use that version of Python automatically when you change into the directory.

Update pyenv like so (note this requires the pyenv-update plugin, installed by default using the method outlined above):

    penv update

By default, installed Python versions will be saved at `~/.pyenv/versions`. To remove a version, simply delete that directory.

# Poetry

pyenv comes with pyenv-virtualenv to manage isolated Python environments. Instead of that, we're going to use **Poetry** to manage virtual environments and dependencies (it has better dependency tree management).

Install Poetry (installs to local user directory, not globally) according to the docs: https://python-poetry.org/docs/#installation

Inside a project directory, initialise the project dependencies:

    poetry init

Add the `pyproject.toml` file to the project repository. Use `poetry new` instead to initialise a brand new project repository instead of `poetry init`.

Set up a new virtual environment for the project:

    poetry install

Add the `poetry.lock` file the the project repository, as this keeps track of installed library versions. Add new project dependencies (this will automatically update `pyproject.toml` and `poetry.lock`):

    poetry add requests
    poetry add Django==3.0.5
    postry add --dev ipython ipdb

List the Poetry-managed virtual environments available to the project:

    poetry env list
    poetry env info

Run python scripts in the virtual environment by preceding them with `poetry run`:

    poetry run python myscript.py

# Update project Python version

Let's say that you want to update the Python version from 3.7 to 3.8, and you're using pyenv and Poetry like you should be. Assuming that you've that you've installed the new Python version using pyenv, carry out the following process to upgrade the version of Python defined in the project `pyproject.toml` file.

Delete the local project venv:

    poetry env list
    poetry env remove <venv name>

Set a new local Python version in the project (using Python 3.8.6 in this example):

    pyenv local 3.8.6

Delete your project poetry.lock file (we'll recreate it in a second anyway), and update your pyproject.toml file to set the new Python version under `[tool.poetry.dependencies]`, e.g.:

    python = "^3.8"

Create a new project venv using Poetry (this should recreate the poetry.lock file):

    poetry install

Test and confirm that your project works with the new version of Python.