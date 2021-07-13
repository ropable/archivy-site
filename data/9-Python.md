---
date: 02-05-21
id: 9
path: ''
tags: []
title: Python
type: note
---

Simple HTTP server:

```python
python3 -m http.server 8080
python -m SimpleHTTPServer 8080
```

New-style formatting examples:

```python
>>> # Format a float to two decimal places
>>> '{:.2f}'.format(3.14159265359)
'3.14'
```

# Datetime & timezones
Simplest way to get now as TZ-aware datetime:

```python
from datetime import datetime, timezone
datetime.now(timezone.utc)
```