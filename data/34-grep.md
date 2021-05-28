---
date: 05-27-21
id: 34
path: ''
tags: []
title: grep
type: note
---

Grep the current location, recursively, ignoring case, printing only files with matches:

```bash
grep -r -i -l <pattern>
grep -ril <pattern>
```

Grep and display 3 lines of context around results:

```bash
grep -C 3 <pattern>
```

Grep using pattern as a regular expression:

```bash
grep -e <pattern>
```