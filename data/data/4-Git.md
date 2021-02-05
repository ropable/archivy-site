---
date: 02-05-21
id: 4
path: ''
tags: []
title: Git
type: note
---

Discard changes to a file (before staging):

```bash
git checkout -- <path>
```

Unstage files (before commit):

```bash
# Unstage one file
git reset -- <path>
# Unstage all staged files
git reset
```

Undo the last commit (before push):

```bash
git commit -m "Something dumb"
# Reset files to HEAD, minus one commit:
git reset --soft HEAD~1
# Edit the files again, use checkout to discard change, etc.
git add <filepath>
git commit -m "New commit"
```

Diff a file between two commits

```bash
# See the difference in requirements.txt between now and one commit back:
git diff HEAD^ HEAD requirements.txt
# See the difference between two specific commits:
git diff <older_sha1> <newer_sha1> requirements.txt
```

Push a specified commit:

```bash
git push <remote name> <commit hash>:<remote branch name>
git push origin 66d22b62t2tf354f354y563y:master
```