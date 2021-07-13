---
date: 06-04-21
id: 37
path: ''
tags:
- python
- ' docker'
title: Jupyter Notebook
type: note
---

# Run a Jupyter Notebook using Docker
Run a notebook, mounting the current directory into the container:

```bash
docker container run --rm -p 8888:8888 -v "$PWD":/home/jovyan/work jupyter/minimal-notebook
```