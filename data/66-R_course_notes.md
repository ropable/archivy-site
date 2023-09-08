---
date: 01-05-23
id: 66
path: ''
tags: []
title: R course notes
type: note
---

Documentation: https://cran.r-project.org/manuals.html

In RStudio, get help using `?<THING>` or `help(<THING>)`:
```
?logical
help(as)
```

# Installing & loading packages
```
available.packages()
install.packages("slidify")
install.packages(c("slidify", "ggplot2", "devtools"))
sessionInfo()

# Comment - load a package:
library(devtools)
```

# Variables
Variable names can contain letters, numbers, dot or underscore. It must start with a letter. Variable assignment is carried out with `=` or with `<-`.

Note that the arrow assignment operator saves variables to a new data object that can be used outside of a function. There is also `<<-` which is the super assignment operator (assigns values to global variables).

Examples:
```
x = 123
foo = "bar"
foo
[1] "bar"
My.Age.Is = 45
My.Age.Is
[1] 45
```

# Datatypes
Show the datatype with `class()`
```
x = 123
class(x)
[1] "numeric"
```
Use `is` to test if an object is a class:
```
foo = 'bar'
is.logical(foo)
[1] FALSE
```
## Types
https://cran.r-project.org/doc/manuals/r-release/R-lang.html#Basic-types

**Numeric / Double**
```
x = 123
y = 2.01
```

**Character**
Either quote type works to assign:
```
foo = "bar"
baz = 'bing'
x = 123
y = as.character(x)
y
[1] "123"
paste(foo, bar)
[1] "bar bing"
substr("Mary had a little lamb", 1, 12)  # Not zero-indexed!
[1] "Mary had a l"
sub("little", "big", "Mary had a little lamb")  # Substitute first occurrence
[1] "Mary had a big lamb"
gsub("a", "@", "Mary had a little lamb")  # Global substituion
[1] "M@ry h@d @ little l@mb"
s = "My name is Shining."
nchar(s)  # String length
[1] 19
input = readline("Give me a string: ")
Give me a string: This is a string
tolower(input)
[1] "this is a string"
toupper(input)
[1] "THIS IS A STRING"
strsplit(input, "")  # Split the elements of a character vector
[[1]]
 [1] "T" "h" "i" "s" " " "i" "s" " " "a" " " "s" "t" "r" "i" "n" "g"
```

**Logical**
`0` evaluates as falsy.
```
x = TRUE
y = FALSE
```

**Vector** - collecton of a fixed size, all holding the same basic data type.
```
foo = c(1, 2, 3)  # Combine values into a Vector or List.
foo = c(foo, 4, 5, 6)  # Append values to existing Vector.
foo = c(foo, "a", "b")  # Coerces all values into strings.
bar = (1:10)  # Create a Vector using colon notation.
baz = seq(2, 20, by=2)  # Create a vector by sequence.
bing = rep("abc", 5)  # Create a vector by replication.
x = seq(1, 20, by=2)
x > 4  # Evaluate vector elements
[1] FALSE FALSE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
x[x>4]  # Return only vector elements evaluating as true.
[1]  5  7  9 11 13 15 17 19
which
match
sort
rev
sample
set.seed
a = c(1,2,3,NA,5,NA,7,8,9,NA)
a
[1]  1  2  3 NA  5 NA  7  8  9 NA
a[c(2:6)]  # Elements 2 through 6
[1]  2  3 NA  5 NA
is.na(a)  # Find if values are missing.
[1] FALSE FALSE FALSE  TRUE FALSE  TRUE FALSE FALSE FALSE  TRUE
which(is.na(a))  # Find the indexes of the NA/missing values.
[1]  4  6 10
sum(is.na(c))  # Count the missing values.
[1] 3
```

**List** - items of variable type and length, can be changed. These a a weird mix of Python-equivalent list and dictionary.
```
l = list("a", "b", "c", 123, TRUE)
> l[1]
[[1]]
[1] "a"
ls = list(first_name="Ashley", surname="Felton", age=44)
ls[1]  # Single bracket will return the name and index value.
$first_name
[1] "Ashley"
ls[[1]]  # Double bracket will return the index value only
[1] "Ashley"
ls$age  # Return an index value by name.
[1] 44
```

**Matrix** - two-dimensional Vector.
```
matrix(0.0, nrow=2, ncol=3)
     [,1] [,2] [,3]
[1,]    0    0   0
[2,]    0    0   0
# Matrix row and columns can be given names:
matrix(c(1:20), ncol=4, dimnames=list(c('r1', 'r2', 'r3', 'r4', 'r5'), c('c1', 'c2', 'c3', 'c4')))
   c1 c2 c3 c4
r1  1  6 11 16
r2  2  7 12 17
r3  3  8 13 18
r4  4  9 14 19
r5  5 10 15 20
# Change nams with rownames() or colnames()
a = diag(42, 3, 4)
> a
     [,1] [,2] [,3] [,4]
[1,]   42    0    0    0
[2,]    0   42    0    0
[3,]    0    0   42    0
a = diag(c(1,2,3,4), 4, 4)
a
     [,1] [,2] [,3] [,4]
[1,]    1    0    0    0
[2,]    0    2    0    0
[3,]    0    0    3    0
[4,]    0    0    0    4

rbind  # Row bind
cbind  # Column bind
```

**Array** - 3+ dimensional Vector.
```
array(0.0, c(2, 3, 2))
, , 1

     [,1] [,2] [,3]
[1,]    0    0    0
[2,]    0    0    0

, , 2

     [,1] [,2] [,3]
[1,]    0    0    0
[2,]    0    0    0
```

**Dataframe** - a table, each column having a header name and holding the same type.

## Conversion
Use `as` to coerce an object to a different class (if possible):
```
as.character(x)
[1] "123"

as.logical
as.integer
as.double
as.complex
as.list   # Accepts only dictionary or vector
```
logical < integer < numeric < complex < character < list

# Comparison/logical operators
```
>
<
==  # EQUALS operator
&   # AND operator
|   # OR operator
!   # NEGATION operator
%in%
```

# Loops and Conditions
```
if (condition) {
  # Run this code
} else if {
  # Run this code instead
} else {
  # Run this code finally
}

ifelse(condition, <evaluate if true>, <evaluate if false>)

while(condition) {
  # Run this code
	next  # Skip to the next iteration
}

for (variable in vector) {
  # Run this iteration
}
```

# Functions
```
my_func = function(var1, var2) {
  print(var1)
	return var2
}

my_func = function(var1, ...) {
  print(var1)
	print(args[1])
}
```

# Pastebin
```
> print("foobar")
[1] "foobar"
> print(paste("Number", 42))
[1] "Number 42"

age = readline("Age? ")
[1] Age? 45
age
[1] "45"
age = as.numeric(readline("Age?   "))
[1] Age?   45
age
[1] 45
```