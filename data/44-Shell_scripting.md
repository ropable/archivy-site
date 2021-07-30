---
date: 07-28-21
id: 44
path: ''
tags: []
title: Shell scripting
type: note
---

# Input handling
Set a default parameter value for an input argument:
```bash
NAME=${1:-you}
```

# String substitution
```bash
echo "One for $NAME, one for me."
```