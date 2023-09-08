---
date: 02-05-21
id: 10
path: ''
tags: []
title: Vim
type: note
---

# Tricks
Delete from cursor to end of line:

```bash
d$
```

Close and open a single fold:

```bash
zc
zo
```

Delete multi-line comment via Visual block mode: `ctrl v`, highlight block, `x`

Insert multi-line comment: `ctrl v`, highlight block, `shift i`, `#`, `Esc`
(wait a second).

Reference: <https://stackoverflow.com/a/1676690/14508>

Replace double quotes with single quotes:

```bash
:%s/"\([^"]*\)"/'\1'/g
```

Record a macro in register **a**:

```bash
qa<commands><ESC>
```

Run a macro (recorded in register **a**):

```bash
@a  # Run it once
99@a  # Run it 99 tmes
```

Reload syntax highlighting:

```bash
:syntax sync fromstart
```

Reference: <https://vim.fandom.com/wiki/Fix_syntax_highlighting>

# Search and replace
In Vim, you can find and replace text using the `:substitute` (`:s`) command. The general form of the substitute command is as follows:
```
:[range]s/{pattern}/{string}/[flags] [count]
```
to search for the first occurrence of the string ‘foo’ in the current line and replace it with ‘bar’, you would use:
```
:s/foo/bar/
```
To replace all occurrences of the search pattern in the current line, add the g flag:
```
:s/foo/bar/g
```
If you want to search and replace the pattern in the entire file, use the percentage character % as a range. This character indicates a range from the first to the last line of the file:
```
:%s/foo/bar/g
```
If the {string} part is omitted, it is considered as an empty string, and the matched pattern is deleted. The following command deletes all instances of the string ‘foo’ in the current line:
```
:s/foo//g
```
## Case sensitivity

By default, the search operation is case sensitive; searching for “FOO” will not match “Foo”. To ignore case for the search pattern, use the i flag:
```
:s/Foo/bar/gi
```
Another way to force ignore case is to append \c after the search pattern. For example, `/Linux\c` performs ignore case search. If you changed the default case setting and you want to perform case sensitive search, use the I flag:
```
:s/foo/bar/gI
```
Uppercase \C after the pattern also forces case match search.

Reference: https://linuxize.com/post/vim-find-replace/