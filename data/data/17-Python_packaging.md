---
date: 02-05-21
id: 17
path: ''
tags: []
title: Python packaging
type: note
---

Install twine via pip. Set up .pyprc similar to the following:

```bash
[distutils]
index-servers=
    pypi
    testpypi
 
[pypi]
username=ashley_felton_dpaw
password=<PASSWORD>
 
[testpypi]
repository: https://test.pypi.org/legacy/
username: ropable
password=<PASSWORD>
```

To create a source distribution:

```bash
python setup.py sdist
```

To register/upload a project on Test Pypi:

```bash
twine upload -r testpypi dist/*
```

Upload source dist to Pypi via twine:

```bash
twine upload dist/*
twine upload dist/filename.tar/gz
```

Install library from a local package (for testing purposes, etc.):

```bash
pip install -e /path/to/setup.py
```
Useful links:

* https://packaging.python.org/guides/using-testpypi/
* https://docs.python.org/3/distutils/packageindex.html#pypirc
* https://packaging.python.org/tutorials/distributing-packages/
* https://packaging.python.org/en/latest/distributing.html#uploading-your-project-to-pypi