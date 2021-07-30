---
date: 02-05-21
id: 10
path: ''
tags: []
title: Vim
type: note
---

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