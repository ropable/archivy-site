---
date: 02-05-21
id: 9
path: ''
tags: []
title: Python
type: note
---

# Profiling / benchmarking
Use the `timeit` module for quick benchmarking.

```bash
$ python -m timeit "from isogram import is_isogram; is_isogram('Emily Jung Schwartzkopf')"
20000 loops, best of 5: 11 usec per loop
```
Or in a Python session:
```bash
In [1]: import timeit
In [2]: timeit.timeit("from isogram import is_isogram; is_isogram('Emily Jung Schwartzkopf')", number=1000)
Out[2]: 0.013915619812905788
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

If we're using Python 3.9+, we can use the `zoneinfo` module:

```python
from zoneinfo import ZoneInfo

datetime.now(ZoneInfo("Australia/Perth"))  # AWST timezone
```

# Tricks
Simple HTTP server:

```python
python3 -m http.server 8080
python -m SimpleHTTPServer 8080
```