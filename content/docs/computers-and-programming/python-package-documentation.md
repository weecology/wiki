---
title: "Python - Package Documentation"
linkTitle: "Python - Package Docs"
summary: " "
---

## Add docs

### Install sphinx and the markdown parser

```sh
pip install sphinx myst-parser 
```

### Make a docs directory and run the quick-start

```sh
mkdir docs
cd docs
sphinx-quickstart
```

### Add markdown and autodoc extensions to `conf.py`

```python
extensions = ['myst_parser',
              'sphinx.ext.autodoc']
```

### Make docs files and edit index.rst

* Edit `index.rst` to include the information you
* Add markdown files for each separate page of docs
* In `index.rst` add the names of the markdown files (without extensions) to the `toctree` block. E.g., if we want to include an the docs in `installation.md` and `getting-started.md`:

```
.. toctree::
   :maxdepth: 2
   :caption: Contents:

   installation
   getting-started
```

### Setup automatic function documentation

```sh
sphinx-apidoc -f  -o source ../<package-name>
```

### Change the theme

* Pick a theme (we currently use either spinx-rtd-theme or furo)
* Install it

```sh
pip install sphinx_rtd_theme
```

* Change the theme value in in `conf.py`

```python
html_theme = "sphinx_rtd_theme"
```

* If using the `sphinx_rtd_theme` also add it to `extensions`

```python
extensions = ['myst_parser',
              'sphinx.ext.autodoc',
              'sphinx_rtd_theme']
```

### Build the docs

```sh
make html
```

## Build docs automatically on readthedocs

Your project should be in an online repository

### Add a docs requirements.txt file

In the docs directory add a `requirement.txt` file that includes the extra packages required for building the docs.

```
myst_parser
sphinx_rtd_theme
```

### Add .readthedocs.yaml

```yaml
version: "2"

build:
  os: "ubuntu-22.04"
  tools:
    python: "3.10"

python:
  install:
    - method: pip
      path: .
    - requirements: docs/requirements.txt

sphinx:
  configuration: docs/conf.py
```

### Connect your GitHub/GitLab account to readthedocs

* Go to <https://readthedocs.com/>
* Click 'Sign up'
* Choose 'Read the Docs Community'
* Click 'Sign up with GitHub' (or GitLab)
* Follow the instructions

### Connect your project

* Go to <https://readthedocs.org/dashboard/>
* Click 'Import a Project'
* If the project is listed select it
* If it is not listed click on 'Import Manually' and provide the requested information

### Enable builds for PRs

If you want to check the doc builds from your PRs enable this by:

1. Go to the project dashboard on readthedocs
2. Select 'Admin'
3. Select 'Advanced Settings'
4. Click 'Build pull requests for this project'
