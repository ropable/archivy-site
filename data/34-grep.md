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
grep -r -i -l PATTERN
grep -ril PATTERN
```

Grep and display 3 lines of context around results:
```bash
grep -C 3 PATTERN
```

Grep using pattern as a regular expression:
```bash
grep -e REGEX_PATTERN
```

Grep case-insensitive, recursive, exclude any of these directories, exclude any of these file extensions:
```bash
grep -ir --exclude-dir={.venv,staticfiles,static,.git,__pycache__} --exclude=*.{pyc,lock} PATTERN
```

References:

- https://www.howtogeek.com/805398/how-to-exclude-patterns-files-and-directories-with-grep/