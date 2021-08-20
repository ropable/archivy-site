---
date: 02-05-21
id: 18
path: ''
tags: []
title: Bash
type: note
---

# User  & group management
To add an existing user account to a group:
```bash
usermod -a -G examplegroup exampleusername
```

To change the primary group a user is assigned to:
```bash
usermod -g groupname username
```

Note the -g here. When you use a lowercase g, you assign a primary group. When you use an uppercase -G , as above, you assign a new secondary group.

To view the groups the current user account is assigned to, run the **groups** command. To view the numerical IDs associated with each group, run the **id** command instead (useful if group name contains spaces):
```bash
groups ashleyf
$ ashleyf : domain users docker
id ashleyf
$ uid=1711209719(ashleyf) gid=1711200513(domain users) groups=1711200513(domain users),998(docker)
```

If you want to view a list of all groups on your system, you can use the **getent** command:
```bash
getent group
```

# Permissions management

Use **chmod** in a more-readable form:
```bash
# chmod {u,g,o,a}{+,-,=}{r,w,x} /path/to/file
chmod u+r /path/to/file  # Give user read access to /path/to/file
```

# Tricks

Every 5 seconds, print the output of a **find** command:
```bash
while sleep 5; do find /path/to/files a -type f -name "buffer.*.log.meta" | wc -l; done
```

Find and print sorted by last modified:
```bash
find . -printf "%T@ %Tc %p\n" | sort -n
```