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

# Strings 
New-style formatting examples:

```python
> # Format a float to two decimal places
> '{:.2f}'.format(3.14159265359)
'3.14'
> # Left-pad an integer with zeroes:
> str(random.randint(0, 999)).zfill(5)
'00123'
```

String interpolation:

```python
def two_fer(name="you"):
    return f"One for {name}, one for me."
```

# Datetime & timezones
Simplest way to get now as TZ-aware datetime:

```python
from datetime import datetime, timezone, timedelta
datetime.now(timezone.utc)  # UTC timezone
datetime.now(timezone(timedelta(hours=8)))  # +0800 timezone
```