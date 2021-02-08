---
date: 02-05-21
id: 10
path: ''
tags: []
title: Vim
type: note
---

Replace double quotes with single quotes:

```vim
:%s/"\([^"]*\)"/'\1'/g
```