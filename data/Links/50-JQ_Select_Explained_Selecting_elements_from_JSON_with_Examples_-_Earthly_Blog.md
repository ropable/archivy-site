---
date: 08-26-21
id: 50
path: Links
tags:
- jq
- ' tools'
- ' reference'
title: JQ Select Explained
type: bookmark
url: https://earthly.dev/blog/jq-select/
---

## What Is JQ

`jq` is a lightweight, command-line JSON processor. I install it with brew (`brew install jq`), but it’s a single portable executable, so it’s easy to install on Linux, Windows, or macOS. To use it, you construct one or more filters, and it applies those filters to a JSON document.

The simplest filter is the identity filter which returns all its input (`.`):
    
    
    $ echo '{"key1":{"key2":"value1"}}' | jq '.'
    
    
    {
      "key1": {
        "key2": "value1"
      }
    }

This filter is handy for just pretty-printing a JSON document.3 I’m going to ignore the pretty-printing and jump right into using `jq` to transform JSON documents.

## Using JQ to Select Elements

I’m going to use `jq` to filter the data returned by the GitHub repository API. The data I get back by default looks like this:
    
    
    $ curl https://api.github.com/repos/stedolan/jq
    
    
    {
      "id": 5101141,
      "node_id": "MDEwOlJlcG9zaXRvcnk1MTAxMTQx",
      "name": "jq",
      "full_name": "stedolan/jq",
      "private": false,
      "owner": {
        "login": "stedolan",
        "id": 79765
      },
      "html_url": "https://github.com/stedolan/jq",
      "description": "Command-line JSON processor",
      "stargazers_count": 19967,
      "watchers_count": 19967,
      "language": "C",
      "license": {
        "key": "other",
        "name": "Other",
        "spdx_id": "NOASSERTION",
        "url": null,
        "node_id": "MDc6TGljZW5zZTA="
      }
    }

`jq` lets us treat the JSON document as an object and select elements inside of it.

Here is how I filter the JSON document to select the value of the `name` key:
    
    
    $ curl https://api.github.com/repos/stedolan/jq | jq ' .name' 
    
    
    "jq"

Similarly, for selecting the value of the `owner` key:
    
    
    $ curl https://api.github.com/repos/stedolan/jq | jq ' .owner' 
    
    
    {
        "login": "stedolan",
        "id": 79765
    }

You can drill in as far as you want like this:
    
    
    $ curl https://api.github.com/repos/stedolan/jq | jq ' .owner.login' 
    
    
    "stedolan"

**What I Learned: Object Identifier-Index**

`jq` lets you select elements in a JSON document like it’s a JavaScript object. Just start with `.` ( for the whole document) and drill down to the value you want. It ends up looking something like this:
    
    
    jq '.key.subkey.subsubkey'

## Using JQ to Select Arrays

If you `curl` the GitHub Issues API, you will get back an array of issues:
    
    
    $ curl https://api.github.com/repos/stedolan/jq/issues?per_page=5    
    
    
    [
      {
        "id": 966024429,
        "number": 2341,
        "title": "Question about license.",
        "body": "I would like to create a [winget](https://github.com/microsoft/winget-cli) package for jq. 🙏🏻"
      },
      {
      
        "id": 962477084,
        "number": 2340,
        "title": "visibility of wiki pages",
        "body": "The visibility of wiki pages to search engines is generally limited; for example, the search result for \"jq Cookbook\" looks like this:"
      },
      {
       
        "id": 955350543,
        "number": 2337,
        "title": "Release 1.6 does not have pre-autoreconf'ed configure script",
        "body": "If you have a usage question, please ask us on either Stack Overflow (https://stackoverflow.com/questions/tagged/jq) or in the #jq channel (http://irc.lc/freenode/%23jq/) on Freenode (https://webchat.freenode.net/)."
      },
      {
        "id": 954792209,
        "number": 2336,
        "title": "Fix typo",
        "body": ""
      },
      {
        "id": 940627936,
        "number": 2334,
        "title": "Compile error messages don't provide column only line number",
        "body": "Compile errors in filter expressions don't include the column number where the parser approximately or exactly locates the error. Most filter expressions are one-liners (are multiple lines even supported?), so the information that the error is on line 1 is not helpful."
      }
    ]

To get a specific element in the array, give `jq` an index:
    
    
    $ curl https://api.github.com/repos/stedolan/jq/issues?per_page=5 | jq '.[4]' 
    
    
     {
        "id": 940627936,
        "number": 2334,
        "title": "Compile error messages don't provide column only line number",
        "body": "Compile errors in filter expressions don't include the column number where the parser approximately or exactly locates the error. Most filter expressions are one-liners (are multiple lines even supported?), so the information that the error is on line 1 is not helpful."
      }

**Side Note: Array Indexing in`jq`**

Array indexing has some helpful convenience syntax.

You can select ranges:
    
    
    $ echo "[1,2,3,4,5]" | jq '.[2:4]'
    
    
    [3,4]

You can select one sided ranges:
    
    
    $ echo "[1,2,3,4,5]" | jq '.[2:]'
    
    
    [3,4,5]

Also, you can use negatives to select from the end:
    
    
    $ echo "[1,2,3,4,5]" | jq '.[-2:]'
    
    
    [4,5]

You can use the array index with the object index:
    
    
    $ curl https://api.github.com/repos/stedolan/jq/issues?per_page=5 | jq '.[4].title' 
    
    
    "Compile error messages don't provide column only line number"

And you can use `[]` to get all the elements in the array. For example, here is how I would get the titles of the issues returned by my API request:
    
    
    $ curl https://api.github.com/repos/stedolan/jq/issues?per_page=5 | jq '.[].title' 
    
    
    "Question about license."
    "visibility of wiki pages"
    "Release 1.6 does not have pre-autoreconf'ed configure script"
    "Fix typo"
    "Compile error messages don't provide column only line number"

**What I Learned: Array-Index**

`jq` lets you select the whole array `[]`, a specific element `[3]`, or ranges `[2:5]` and combine these with the object index if needed.

It ends up looking something like this:
    
    
    jq '.key[].subkey[2]

**Side Note: Removing Quotes From JQ Output**

The `-r` option in `jq` gives you raw strings if you need that.
    
    
    $ echo '["1","2","3"]' | jq -r '.[]'
    1
    2
    3

The `-j` option (for join) can combine together your output.
    
    
    $ echo '["1","2","3"]' | jq -j '.[]'
    123

## Putting Elements in an Array using `jq`

Once you start using the array index to select elements, you have a new problem. The data returned won’t be a valid JSON document. In the example above, the issue titles were new line delimited:
    
    
    "Question about license."
    "visibility of wiki pages"
    "Release 1.6 does not have pre-autoreconf'ed configure script"
    "Fix typo"
    "Compile error messages don't provide column only line number"
    ...

In fact, whenever you ask `jq` to return an unwrapped collection of elements, it prints them each on a new line. You can see this by explicitly asking `jq` to ignore its input and instead return two numbers:
    
    
    $ echo '""' | jq '1,2' 
    1
    2

You can resolve this the same way you would turn the text `1,2` into an array in JavaScript: By wrapping it in an array constructor `[ ... ]`.
    
    
    $ echo '""' | jq '[1,2]' 
    
    
    [
      1,
      2
    ]

Similarly, to put a generated collection of results into a JSON array, you wrap it in an array constructor `[ ... ]`.

My GitHub issue title filter (`.[].title`) then becomes `[ .[].title ]` like this:
    
    
    $ curl https://api.github.com/repos/stedolan/jq/issues?per_page=5 | \
      jq '[ .[].title ] ' 
    
    
    [
      "Question about license.",
      "visibility of wiki pages",
      "Release 1.6 does not have pre-autoreconf'ed configure script",
      "Fix typo",
      "Compile error messages don't provide column only line number"
    ]

Now I have a valid JSON document.

**What I Learned: Array Constructors**

If your `jq` query returns more than one element, they will be returned newline delimited.
    
    
    $ echo '[{"a":"b"},{"a":"c"}]' | jq -r '.[].a'
      "b"
      "c"

To turn these values into a JSON array, what you do is similar to creating an array in JavaScript: You wrap the values in an array constructor (`[...]`).

It ends up looking something like this:
    
    
    $ echo '[{"a":"b"},{"a":"c"}]' | jq -r '[ .[].a ]'
    
    
    [
      "b",
      "c"
    ]

## Using `jq` to Select Multiple Fields

The GitHub issues API has a lot of details I don’t care about. I want to select multiple fields from the returned JSON document and leave the rest behind.

The easiest way to do this is using `,` to specify multiple filters:
    
    
    $ curl https://api.github.com/repos/stedolan/jq/issues?per_page=2 | \ 
      jq ' .[].title, .[].number'
    
    
    "Question about license."
    "visibility of wiki pages"
    2341
    2340

But this is returning the results of one selection after the other. To change the ordering, I can factor out the array selector:
    
    
    $ curl https://api.github.com/repos/stedolan/jq/issues?per_page=2 | \
      jq '.[] |  .title, .number'
    
    
    "Question about license."
    2341
    "visibility of wiki pages"
    2340

This refactoring uses a pipe (|), which I’ll talk about shortly, and runs my object selectors (`.title` and `.number`) on each array element.

If you wrap the query in the array constructor you get this:
    
    
    $ curl https://api.github.com/repos/stedolan/jq/issues?per_page=2 | \
      jq '[ .[] |  .title, .number ]'
    
    
    [
      "Question about license.",
      2341,
      "visibility of wiki pages",
      2340
    ]

But this still isn’t the JSON document I need. To get these values into a proper JSON object, I need an object constructor `{ ... }`.

## Putting Elements Into an Object Using `jq`

Let’s look at some simple examples before showing how my GitHub query can use an object constructor.

### A Little Example

I have an array that contains my name (`["Adam","Gordon","Bell"]`), and I want to turn it into a JSON object like this:
    
    
    {
      "first_name":"Adam",
      "last_name":"Bell"
    }

I can select the elements I need using array indexing like this:
    
    
    $ echo '["Adam","Gordon","Bell"]' | jq -r '.[0], .[2]'
    Adam
    Bell

To wrap those values into the shape I need, I can replace the values with the array indexes that return them:
    
    
    {
      "first_name":.[0],
      "last_name":.[2]
    }

Or on a single line like this:
    
    
    $ echo '["Adam","Gordon","Bell"]' | jq -r '{ "first_name":.[0], "last_name": .[2]}'
    
    
    {
      "first_name": "Adam",
      "last_name": "Bell"
    }

This syntax is the same syntax for creating an object in a JSON document. The only difference is you can use the object and array queries you’ve built up as the values.

### Back to GitHub

Returning to my GitHub API problem, to wrap the number and the title up into an array I use the object constructor like this:
    
    
    $ curl https://api.github.com/repos/stedolan/jq/issues?per_page=2 | \
      jq '[ .[] | { title: .title, number: .number} ]'
    
    
    [
      {
        "title": "Question about license.",
        "number": 2341
      },
      {
        "title": "visibility of wiki pages",
        "number": 2340
      }
    ]

**What I Learned: Object Constructors**

To put the elements you’ve selected back into a JSON document, you can wrap them in an object constructor `{ ... }`.

If you were building up a JSON object out of several selectors, it would end up looking something like this:
    
    
    jq '{ "key1": <<jq filter>>, "key2": <<jq filter>> }'

Which is the same syntax for an object in a JSON document, except with `jq` you can use filters as values.4

## Sorting and Counting With JQ

The next problem I have is that I want to summarize some this JSON data. Each issue returned by GitHub has a collection of labels:
    
    
    $ curl https://api.github.com/repos/stedolan/jq/issues/2289 | \   
      jq ' { title: .title, number: .number, labels: .labels} '
    
    
      {
        "title": "Bump jinja2 from 2.10 to 2.11.3 in /docs",
        "number": 2289,
        "labels": [
          "feature request",
          "dependencies"
        ]
      }

### `jq` Built-in Functions

If I want those labels in alphabetical order I can use the built in `sort` function. It works like this:
    
    
    $  echo '["3","2","1"]' | jq 'sort'
    ["1", "2", "3"]

This is similar to how I would sort an array in JavaScript:
    
    
    const l = ["3","2","1"];
    l.sort();

Other built-ins that mirror JavaScript functionality are available, like `length`, `reverse`, and `tostring` and they can all be used in a similar way:
    
    
    $  echo '["3","2","1"]' | jq 'reverse'
    ["1", "2", "3"]
    $  echo '["3","2","1"]' | jq 'length'
    3

If I can combine these built-ins with the selectors I’ve built up so far, I’ll have solved my label sorting problem. So I’ll show that next.

**What I Learned:`jq` built-ins**

`jq` has many built-in functions. There are probably too many to remember but the built-ins tend to mirror JavaScript functions, so give those a try before heading to [jq manual](https://stedolan.github.io/jq/manual/#Builtinoperatorsandfunctions) , and you might get lucky.5

### Pipes and Filters

Before I can use `sort` to sort the labels from my GitHub API request, I need to explain how pipes and filters work in `jq`.

`jq` is a filter in the UNIX command line sense. You pipe (`|`) a JSON document to it, and it filters it and outputs it to standard out. I could easily use this feature to chain together jq invocations like this:
    
    
    echo '{"title":"JQ Select"}' | jq '.title' | jq 'length'
    9

This is a wordy, though simple, way to determine the length of a string in a JSON document. You can use this same idea to combine various `jq` built-in functions with the features I’ve shown so far. But there is an easier way, though. You can use pipes inside of `jq` and conceptually they work just like shell pipes:
    
    
    echo '{"title":"JQ Select"}' | jq '.title | length'
    9

Here are some more examples:

  * `.title | length` will return the length of the title
  * `.number | tostring` will return the issue number as a string
  * `.[] | .key` will return the values of key `key` in the array (this is equivalent to this `.[].key`)



This means that sorting my labels array is simple. I can just change `.labels` to `.labels | sort`:
    
    
    $ curl https://api.github.com/repos/stedolan/jq/issues/2289 | \
      jq ' { title: .title, number: .number, labels: .labels | sort } '    
    
    
      {
        "title": "Bump jinja2 from 2.10 to 2.11.3 in /docs",
        "number": 2289,
        "labels": [
          "dependencies",
          "feature request"
        ]
      }

And if you want just a label count that is easy as well:
    
    
    $ curl https://api.github.com/repos/stedolan/jq/issues/2289 | \
      jq ' { title: .title, number: .number, labels: .labels | length } '    
    
    
      {
        "title": "Bump jinja2 from 2.10 to 2.11.3 in /docs",
        "number": 2289,
        "labels": 2
      }

**What I Learned: Pipes and Filters**

Everything in `jq` is a filter that you can combine with pipes (`|`). This mimics the behavior of a UNIX shell.

You can use the pipes and the `jq` built-ins to build complicated transformations from simple operations.

It ends up looking something like this:
    
    
    jq ' .key1.subkey2[] | sort ' # sorting
    jq ' .key2.subkey | length' # length of string or array
    jq ' .key3 | floor | tostring | length' # and so on

## Maps and Selects Using JQ

The issues list I was looking at has many low-quality issues in it.6 Let’s say I want to grab all the items that are labeled. This would let me skip all the drive-by fix-my-problem issues.

Unfortunately, it’s impossible to do this with the GitHub API unless you specify all the possible labels in your query. However, I can easily do this query on the command line by filtering our results with `jq`. However, to do so, I’m going to need a couple more `jq` functions.

My query so far looks like this:
    
    
      jq '[ .[] | { title: .title, number: .number, labels: .labels | length } ]'

The first thing I can do is simplify it using `map`.
    
    
      jq 'map({ title: .title, number: .number, labels: .labels | length }) 

`map(...)` let’s you unwrap an array, apply a filter and then rewrap the results back into an array. You can think of it as a shorthand for `[ .[] | ... ]` and it comes up quite a bit in my experience, so it’s worth it committing to memory.

I can combine that with a select statement that looks like this:
    
    
    map(select(.labels > 0))

`select` is a built-in function that takes a boolean expression and only returns elements that match. It’s similar to the `WHERE` clause in a SQL statement or array filter in JavaScript.

Like `map`, I find `select` comes up quite a bit, so while you may have to come back to this article or google it the first few times you need it, with luck, it will start to stick to your memory after that.

Putting this all together looks like this:
    
    
    curl https://api.github.com/repos/stedolan/jq/issues?per_page=100 | \
       jq 'map({ title: .title, number: .number, labels: .labels | length }) | 
       map(select(.labels > 0))'
    
    
    [
      {
        "title": "Bump lxml from 4.3.1 to 4.6.3 in /docs",
        "number": 2295,
        "labels": 1
      },
      {
        "title": "Bump pyyaml from 3.13 to 5.4 in /docs",
        "number": 2291,
        "labels": 1
      },
      {
        "title": "Bump jinja2 from 2.10 to 2.11.3 in /docs",
        "number": 2289,
        "labels": 1
      },
      {
        "title": "Debugging help through showing pipeline intermediates. ",
        "number": 2206,
        "labels": 1
      }
    ]

This uses three object indexes, two maps, two pipes, a `length` function, and a `select` predicate. But if you’ve followed along, this should all make sense. It’s all just composing together filters until you get the result you need.

Now lets talk about how you can put this knowledge into practice.

## In Review

**What I Learned**

Here is what I’ve learned so far:

`jq` lets you select elements by starting with a `.` and accessing keys and arrays like it’s a JavaScript Object (which is it is). This feature uses the Object and Array index `jq` creates of a JSON document and look like this:
    
    
    jq '.key[0].subkey[2:3].subsubkey'

`jq` programs can contain object constructors `{ ... }` and array constructors `[ ... ]`. You use these when you want to wrap back up something you’ve pulled out of a JSON document using the above indexes:
    
    
    jq '[ { key1: .key1, key2: .key2 }  ]'

`jq` contains built-in functions (`length`,`sort`,`select`,`map`) and pipes (`|`), and you can compose these together just like you can combine pipes and filters at the command line:
    
    
      jq 'map({ order-of-magitude: .items | length | tostring | length }) 

### Next Steps for Mastering `jq`

Reading about (or writing about) a tool is not enough to master it. Action is needed. Here is my process for cementing this knowledge:

#### 1\. Complete the `jq` Tutorial

[jq-tutorial](https://github.com/rjz/jq-tutorial) is not a tutorial at all, but a collection of around 20 interactive exercises that test you knowledge of `jq`. I’ve found it extremely helpful.

#### 2\. Try Using Your Memory First

Whenever I need to extract data or transform a JSON document, I try to do it first without looking anything up. If memory fails me, sometimes [jqterm](https://jqterm.com/), which has auto-completion, is helpful. Often, I still need to look something up, but science has shown that [repeated retrieval yields retention](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3983480/). So over time, my retention should improve.

#### 3\. Use It

If you don’t use a tool, you will never master it. So when I have a task that can be solved using `jq`, then it is what I use. At least for the next little while, even if there is an easier way to do it. Whether it’s exploring a REST API or looking at `docker inspect` results, JSON is everywhere, so opportunities abound.

#### 4\. Learn More

Lastly, to deepen my knowledge, I’m learning about recursive descent, declaring variables, and defining functions and advanced features found in the [manual](https://stedolan.github.io/jq/manual/#Advancedfeatures). Of course, these things rarely come up, but after writing all this, I’m hooked on this tool.

Doing all this isn’t necessary, but if you follow me in some of these steps, I think using `jq` will become second nature.