---
date: 02-05-21
id: 4
path: ''
tags: []
title: Git
type: note
---

# Common operations
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
git diff <OLDER COMMIT> <NEWER COMMIT> requirements.txt
# Get more lines of context around a diff:
git diff -U10
```

See the changes made for one particular commit:
```bash
git show <SHA HASH>
```

Push a specified commit:

```bash
git push <remote name> <commit hash>:<remote branch name>
git push origin 66d22b62t2tf354f354y563y:master
```

Merge from another branch without commiting:

```bash
git merge --no-commit --no-ff otherbranch
```

Abort an in-progress merge:
```bash
git merge --abort
```

Commit changes to a new branch:

```bash
git checkout -b new-branch
git add <files>
git commit -m <message> 
```

Move your last commits to another branch:

```bash
# Create a new branch
git branch feature/newbranch

# This will create a new branch including all of the commits of the current branch.
# Move the current branch back two commits
git reset --keep HEAD~2

# Checkout the new branch
git checkout feature/newbranch
```

# Resolving a merge conflict

Merge conflicts look like this in files:
```bash
If you have questions, please
<<<<<<< HEAD
open an issue
=======
ask your question in IRC.
>>>>>>> branch-a
```

 Changes from the HEAD or base branch are after the line `<<<<<<< HEAD`. Next, you'll see `=======`, which divides your changes from the changes in the other branch, followed by `>>>>>>> BRANCH-NAME`.
 
 # Errors
 
 Broken pipe:

```bash
$ git push
Counting objects: 1254, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (273/273), done.
packet_write_wait: Connection to 13.237.44.5 port 22: Broken pipe
fatal: The remote end hung up unexpectedly
```
Try to to increase the HTTP post buffer size to allow for larger chunks to be pushed up to the remote repo:
```
git config http.postBuffer 52428800
```