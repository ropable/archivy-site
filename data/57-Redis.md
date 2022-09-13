---
date: 06-28-22
id: 57
path: ''
tags: []
title: Redis
type: note
---

# Commands

```
# Set the foo key to value "bar":
> set foo bar  
OK
> get foo
"bar"
# Set a value that expires after 10 seconds:
> set cachedval foobar ex 10
OK
> get cachedvale
"foobar"
> get cachedvale
(nil)
# Another way to do this:
setex cachedvalue 10 foobar
# Set a key value only if it doesn't already exist:
> setnx foo baz
(integer) 0
# Increment an integer key value:
> incr count
1
> incr count
2

# Get a set of all keys:
keys '*'
```