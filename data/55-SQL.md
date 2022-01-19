---
date: 11-24-21
id: 55
path: ''
tags: []
title: SQL
type: note
---

Delete from SELECT clause:

```sql
DELETE FROM table1
WHERE rowid IN (SELECT rowid FROM table1 WHERE foo = 'bar');
```