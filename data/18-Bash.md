---
date: 02-05-21
id: 18
path: ''
tags: []
title: Bash
type: note
---

Use chmod in a more-readable form:

```bash
chmod OPTIONS {u,g,o}{+,-,=}{r,w,x} /path/to/file
```

Find a user's groups and group ID (useful if group name contains spaces):

```bash
id ashleyf
# uid=1711209719(ashleyf) gid=1711200513(domain users) groups=1711200513(domain users),998(docker)
```

Every 5 seconds, print the output of a find command:

```bash
while sleep 5; do find /path/to/files a -type f -name "buffer.*.log.meta" | wc -l; done
```