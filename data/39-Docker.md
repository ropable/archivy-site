---
date: 06-17-21
id: 39
path: ''
tags: []
title: Docker
type: note
---

Build a Dockerfile using a different Dockerfile:

```bash
docker image build --file Dockerfile.source --tag ropable/archivy .
docker image build -f Dockerfile.source -t ropable/archivy .
```