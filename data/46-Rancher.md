---
date: 08-02-21
id: 46
path: ''
tags:
- kubernetes
- ' k3s'
title: Rancher
type: note
---

Allow writing to the kubeconfig file (ref https://rancher.com/docs/k3s/latest/en/installation/install-options/server-config/#client-options):

```bash
curl -sfL https://get.k3s.io | sh -s - --write-kubeconfig-mode 644
```