---
date: 05-28-21
id: 35
path: Links
tags:
- shell
title: Shell Field Guide
type: bookmark
url: https://raimonster.com/scripting-field-guide/
---

# Shell Field Guide

## Table of Contents

  * 1\. Introduction
  * 2\. Which Shell?
  * 3\. Level
  * 4\. Patterns
    * 4.1. Use Shellcheck
    * 4.2. Overview
    * 4.3. Booleans and Conditionals
    * 4.4. Arrays
    * 4.5. Slurping arrays
    * 4.6. Functions
    * 4.7. Variables
    * 4.8. Variable Expansions
    * 4.9. Interpolation
    * 4.10. dispatch functions using args
    * 4.11. command_not_found_handle
    * 4.12. Return Values for Conditionals
    * 4.13. set variable in an "if" test
    * 4.14. Do work on loop conditions
    * 4.15. One Branch Conditionals
    * 4.16. pushd/popd
    * 4.17. wrap functions
    * 4.18. use [[
    * 4.19. eval?
    * 4.20. pass commands around
    * 4.21. The Toplevel Is Hopeless
    * 4.22. Check your deps
    * 4.23. source files
    * 4.24. Use Scripts as a Libs
    * 4.25. Tmpfiles Everywhere
    * 4.26. Cleanup tasks with trap
    * 4.27. array of callbacks on_exit
    * 4.28. stacktrace on error
    * 4.29. Dots and colons allowed in function names!
    *     * 4.31. do_times/foreach_*
    * 4.32. <(foo) and >(foo)
    * 4.33. Use xargs
    * 4.34. change loops for "mapping/reducing" functions
      * 4.34.1. find
      * 4.34.2. grep -Fvf
    * 4.35. pass flags as a splatted array
    * 4.36. inherit_errexit
    * 4.37. GNU Parallel
    * 4.38. HEREDOCS
  * 5\. Interactive
    * 5.1. Save your small scripts
    * 5.2. Increased Interactivity
    * 5.3. Aliases
    * 5.4. functions can generate aliases
    * 5.5. Override (advise?) common functions
    * 5.6. Faster iteration on pipes
    * 5.7. Use ${var?error msg} on templates
  * 6\. Debugging
    *     * 6.2. DRY_RUN
    * 6.3. Cheap debugging flag
    * 6.4. explore a pipe with tee >(some_command) |
    * 6.5. tee+sudo
    * 6.6. quoting
  * 7\. zsh-only
    * 7.1. Word spliting
    * 7.2. globbing
    * 7.3. Some global aliases:
    * 7.4. Expansion of global aliases
    * 7.5. Autocomplete
    * 7.6. Create helpers and generate global aliases automagically
    * 7.7. suffix aliases don't have to match a filename
    * 7.8. noglob
    * 7.9. make noglob 'transparent' to bash
    * 7.10. glob nested expansion
    * 7.11. Some extra shortcuts for nice things
    * 7.12. =()
  *     * 8.1. just use cat/netcat/pipes with <()
      * 8.1.1. what's the unifying theory behind all that?
    * 8.2. redirects & streams
    * 8.3. The $0 pattern
    * 8.4. use git staging area to diff outputs of commands
    * 8.5. coprocs
  * 9\. links
  * 10\. From shell to lisp and everything in between
  * 11\. Credits



## 1 Introduction

This booklet is intended to be a catalog of tricks and techniques you may want to use if you're doing some sort of complex scripting. Some are just useful, some are more playful, and might not have such direct impact in your day-to-day life. Some are pure entertainment. You'll have to judge by yourself which things belong to which category. I'll try to keep the rethoric to the minimum to maximize signal/noise. 

The git repo is at <https://github.com/kidd/scripting-field-guide/>. Any feedback is greatly appreciated. Keep in mind this is not any kind of official doc. I just write MY current "state of the art" and I'll be updating the contents with useful stuff I find or discover, that are not widely explained in usual manuals/wikis. 

You probably have some amount of sh/bash/zsh in your stack that probably started as 1 off scripts, and probably later on started growing and being copypasted everywhere in your pipelines, or your coworkers use for their own use (with some variations), etc. Those scripts are very difficult to kill and they have a very high mutation rate. 

## 2 Which Shell?

No matter if you use Linux, Mac, or Windows, you should be living most of the time in a shell to enjoy the content shown here. Some value comes from the automated scripts, and some comes from the daily usage and refinement of your helper functions, aliases, etc. in interactive mode. 

In general the examples here are ment to run in Bash or Zsh, which are compatible for the most part. 

## 3 Level

These examples are based on non-trivial real world code I've written using patterns I haven't seen applied in many places over the net. A few of the snippets are stolen from public repos I find interesting. Also, important scripting stuff might be missing if I don't feel I have anything to add to the generally available info around. 

## 4 Patterns

### 4.1 Use Shellcheck

First, let's get that out of the way. This is low hanging fruit. And you will get the most of this booklet by following it. 

A lot of the most common errors we usually make are well known ones. And in fact, we all usually fail in similar ways. Bash is known for being error prone when dealing with testing variable values, string operations, or flaky subshells and pipes. 

Installing [shellcheck](https://www.shellcheck.net/) will flag you many of those ticking bombs. 

No matter which editor you are using, but you should be able to install a plugin to do automatic checks while you're editing. 

In emacs' case, the package is called [flymake-shellcheck](https://github.com/federicotdn/flymake-shellcheck), and a quick configuration for it is: 
    
    
    (use-package flymake-shellcheck
      :ensure t
      :commands flymake-shellcheck-load
      :init
      (add-hook 'sh-mode-hook 'flymake-shellcheck-load))
    

Shellcheck is available on most distros, so it's just an `apt`, `brew`, or `nix-env` away. 

### 4.2 Overview

In this section, we're covering the parts of the basics that are not so basic after all, or that are more unique in shellscripting languages. 

### 4.3 Booleans and Conditionals

In any shell, `foo && bar` will execute `bar` only if `foo` succeeded. That means that `foo` returned 0. That means that to && (which you read like "and"), 0 is true. so yes. 0 is true, and other values are false. 

### 4.4 Arrays

Ordered list of things. 
    
    
    foo=("ls" "/tmp/")
    
    echo ${foo[-2]}
    echo ${foo[-1]}
    echo ${foo[0]}
    echo ${foo[1]}
    echo ${foo[2]}
    
    for i in "${foo[@]}"; do
      echo $i
    done
    
    $foo
    ${foo[*]}
    ${foo[@]}
    echo ${#foo[*]}
    echo ${#foo[@]}
    

Are `*` and `@` equal? [nope](https://stackoverflow.com/questions/2761723/what-is-the-difference-between-and-in-shell-scripts). 
    
    
    "${foo[@]}"
    "${foo[*]}"
    

### 4.5 Slurping arrays

A nice way to read a bunch of elements from one go is to use `readarray`. 
    
    
    parse_args() {
      [[ $# -eq 0 ]] && die "Usage: $0 <version>"
      version="$1"
      local version_split=$(echo $version | tr '.' '\n')
      readarray -t version_array <<< "$version_split"
    
      if [[ -z ${version_array[3]} ]]; then
        die "not enough version numbers"
      fi
    }
    

Even nicer would be to use IFS so we'd be able to split in one go. 
    
    
    IFS=. read -a ver <<<"1.23.1.0"
    echo ${ver[0]}
    echo "next/${ver[0]}.${ver[1]}.x.x"
    

Or, use it in a destructuring fashion: 
    
    
    get_nix_version_parts(){
      local major minor patch
      # shellcheck disable=SC2034,SC2162
      IFS="." read major minor patch < <(get_nix_version)
      local -p
    }
    
    $ get_nix_version_parts
    major=2
    minor=3
    patch=4
    

<https://news.ycombinator.com/item?id=24408318>

### 4.6 Functions

Functions are functions. They receive arguments, and they return a value. 

The special thing about functions in shell is that they also can use the file descriptors of the process. That means that they "inherit" STDIN, STDOUT, STDERR (and maybe more). 

Use them. 

Another point is that function names can be passed as parameters, because they are passed as strings, but you can call them inside as functions again. 
    
    
    f() {
      $1 hi
    }
    
    f echo
    f touch # will create a file 'hi'
    

### 4.7 Variables

By default variables are global, to a file. No matter if you assign them for the first time inside a function, or at the top level. 
    
    
    foo=3
    bar=$foo
    f() {
      echo $bar
    }
    f
    
    
    
    f() {
      bar=1
    }
    f
    echo $bar
    

You make a variable local to a function with `local`. Use it as much as you can (kinda obvious). 
    
    
    myfun() {
      local bar
      bar=3
      echo $bar
    }
    
    bar=4
    echo $bar
    myfun
    echo $bar
    

### 4.8 Variable Expansions

They offer some variable manipulations using shell only, not having to create another process `sed,awk,perl`. 
    
    
    v=banana
    # substitute one
    echo ${v/na/NA}   # baNAna
    # substitute many
    echo ${v//na/NA}  # baNANA
    
    # substitute from the start (think ^ in PCRE)
    echo ${v/#ba/NA}  # NAnana
    
    # substitute from the end
    echo ${v/%na/NA}  # banaNA
    

Take a read on <https://tldp.org/LDP/abs/html/manipulatingvars.html> and <https://www.gnu.org/software/bash/manual/html_node/Shell-Parameter-Expansion.html> for more details. 

And a nice non-obvious trick from here is to prefix or suffix a variable string: 
    
    
    v=banana
    echo ${v/%/na}   # bananana
    echo ${v/#/na}   # nabanana
    

And a less obvious trick is to prefix every element of an array with a fixed string: 
    
    
    local arr=(var1=1 var2=2)
    echo ${arr[*]/#/"--env "}
    

This will produce `--env var1=1 --env var2=2`. Super useful to be combined when building flags for docker. 

### 4.9 Interpolation

We previously saw that functions can be passed around as strings, and be called later on. 

Something that might not be obvious is that the string can be created from shorter strings, and that allows for an extra flexibility, that comes with its own dangers, but it's a very useful pattern to dispatch functions based on user input or function outputs. 
    
    
    l=l
    s=s
    $l$s .
    

### 4.10 dispatch functions using args

A nice usage of the previous technique is using user's input as a dispatching method. 

You've probably seen this pattern already: 
    
    
    while [[ $# -gt 0 ]]; do
    
    case $1 in
      foo)
        foo
        ;;
      *)
        exit 1
        ;;
    esac
    shift
    done
    

And it is useful for its own good, and flexible. 

But for some simpler cases, we can dispatch based on the variable itself: 
    
    
    cmd_foo() {
     do-something
    }
    
    cmd_$1
    

The problem with this is that in case we supply a `$1` that doesn't map to any `cmd_$1` we'll get something like 
    
    
    bash: cmd_notexisting: command not found
    

### 4.11 command_not_found_handle

Here's a detail on a kinda obscure bash (only bash) feature. 

You can set a hook that will be called when bash tries to run a command and it doesn't find it. 
    
    
    command_not_found_handle() {
      echo "$1 is not a correct command. Cmds allowed:"
      echo "$(typeset -F | grep cmd_ | sed -e 's/.*cmd_/cmd_/')"
    }
    
    cmd_foo() {
      echo "foo"
    }
    
    cmd_baz() {
      echo "baz"
    }
    cmd_bar
    

you can unset the function `command_not_found_handle` to go back to the normal behavior. 

### 4.12 Return Values for Conditionals

`if` 's test condition can use the return values of commands. That's a known thing, but lots of code you see around rely on `[[]]` to test the return values of commands/functions anyway. 
    
    
    if echo "foo" | grep "bar" ; then
      echo "found!"
    fi
    

This is much clearer than 
    
    
    if [[ ! -z $( echo "foo" | grep "bar") ]]; then
      echo "found!"
    fi
    

As easy and trivial as it seems, this way of thinking pushes you forward to thinking on creating smaller functions that check the conditions and `return` 0 or non 0. It's syntactically smaller, and usually makes you play by the rules of the commands, more than just finding your way around the output strings. 
    
    
    if less_than $package "1.3.2"; then
      die "can't proceed"
    fi
    

### 4.13 set variable in an "if" test

Usual pattern to capture the output of a command and branch depending on its return value is: 
    
    
    res="$(... whatever ...)"
    if [ "$?" -eq 0 ]; then ...
                            ...
    fi
    

Well, you can test the return value AND capture the output at the same time! 
    
    
    if res="$(...)"; then
      ...
    fi
    

Unfortunately, it doesnt' work with `local`, so you can't be defining a local var in the same line. So, the variable is either global, or you spent a line to declare it local before. Still, I think I prefer to have a line to declare the variable as local rather than having explicit `$?`'s around. 
    
    
    local var1
    if var1=$(f); then
      echo "$var1"
    fi
    

Ref: <https://news.ycombinator.com/item?id=27163494>

### 4.14 Do work on loop conditions

I've not seen it used a lot (and there might be a reason for it, who knows), `while` conditions are just plain commands, so you can put other stuff than `[]/[[]]/test` there. 

Heres's an idiomatic way to iterate through all the arguments of a function while consuming the `$*` array. 
    
    
    while(($#)) ; do
      #...
      shift
    done
    

And here's a pseudo-repl that keeps shooting one-off commands. This will keep shooting `tr` commands to whatever strings you give it, with the usual rlwrap goodies. 
    
    
    while rlwrap -o -S'>> ' tr a-z A-Z ; do :; done
    

Note: `:` is a nop builtin in bash. 

### 4.15 One Branch Conditionals

The usual conditionals one sees everywhere look like `if`. 
    
    
    if [[ some-condition ]]; then
      echo "yes"
    fi
    

This is all good and fine, but in the same vein of using the least powerful construct for each task, it's nice to think of the one way conditionals in the form of `&&` and `||` as a way to explicitly say that we don't want to do anything else when the condition is not met. It's a hint to the reader. 
    
    
    some-condition || {
       echo "log: warning!"
    }
    
    other-condition && {
       echo "log: all cool"
    }
    

This conveys the intention of doing something **just** in one case, and that the negation of this is not interesting at all. There's a big warning you have to be aware of. The same as with lua's `... and .. or ..`, bash `||` and `&&` are not interchangeable for `if...else...end`. [BashWiki](https://mywiki.wooledge.org/BashPitfalls#cmd1_.26.26_cmd2_.7C.7C_cmd3) has an explanation why, but, the same as in Lua's case, if the "then" part returns false, the else will run. 

There are lots of references to this, but I like this recent post where it explains it for arrays in higher level languages like ruby: <https://jesseduffield.com/array-functions-and-the-rule-of-least-power/>

An extended article of this kind of conditionals can be found [here](https://timvisee.com/blog/elegant-bash-conditionals/). 

### 4.16 pushd/popd

pushd and popd are used to move to some directory and go back to it in a stack fashion, so nesting can happen and you never lose track. The problem is that it still is on you to have a `popd` per `pushd`. 
    
    
    pushd /tmp/my-dir
      echo $PWD
    popd
    

Here's an alternative way, that at least makes sure that you close all pushd with a popd. 

Starting a new shell and cd-ing , will make all commands in that subshell be in that directory, and will come back to the old directory after closing the new spawned shell. 
    
    
    (cd /tmp/my-dir
      ls
    )
    

Remember to `inherit_errexit` or `set -e` inside the subshell if you need. That's a very easy trap to fall into. 

### 4.17 wrap functions

Bash can't pass blocks of code around, but the alternative is to pass functions. More on that later. 
    
    
    mute() {
      $@ >/dev/null
    }
    
    mute ls
    

### 4.18 use [[

Unless you want your script to be POSIX compliant, use `[[` instead of `[`. `[` is a regular command. It's like `ls`, or `true`. You can check it by searching for a file named `[` in your path. 

Being a normal command it always evaluates its params, like a regular function. On the other hand though, `[[` is a special bash operator, and it evaluates the parameters lazily. 
    
    
    # [[ does lazy evaluation:
    [[ a = b && $(echo foo >&2) ]]
    
    # [ does not:
    [ a = b -a "$(echo foo >&2)” ]
    

Ref: <https://lists.gnu.org/archive/html/help-bash/2014-06/msg00013.html>

### 4.19 eval?

When you have mostly small functions that are mostly pure, you compose them like you'd do in any other language. 

In the following snippet, we are in a release script. Some step builds a package inside an image, another step tests a package already built. 

A nice way to build ubuntus, for example, is to add an ARG to the Dockerfile so we can build several ubuntu versions using the same file. 

It'd look something like this: 
    
    
    ARG VERSION
    FROM ubuntu:$VERSION
    
    RUN apt-get ...
    ...
    

We build that image and do all the building inside it, mounting a volume shared with our host, so we can extract our `.deb` file easily. 

After that, to do some smoke tests on the package, the idea is to install the `.deb` file in a fresh ubuntu image. 

Let's pick the same base image we picked to build the package. 
    
    
    # evaluate the string "centos:$VERSION" (that comes from
    # centos/Dockerfile) in the current scope
    local VERSION=$(get_version $DISTRO)
    run_test "file.deb" "$(eval echo $(awk '/^FROM /{print $2; exit}' $LOCAL_PATH/$(get_dockerfile_for $DISTRO)))"
    

The usage of eval is there to interpolate the string that we get from the `FROM` in the current environment. 

WARNING: You know, anything that uses `eval` is dangerous per se. Do not use it unless you know very well what you're doing AND the input is 100% under your control. Usually, more restricted commands can achieve what you want to do. In this particular case, you could use `envsubst`, or just manually replace `$\{?VERSION\}?` in a sed. 
    
    
    test_release "$PACKAGE_PATH" $(awk '/^FROM /{print $2; exit}' $LOCAL_PATH/$(get_dockerfile_for $DISTRO) | sed -e "s/\$VERSION/$VERSION/")
    

Yet another way is using [shell parameter expansions.](https://www.gnu.org/software/bash/manual/html_node/Shell-Parameter-Expansion.html)
    
    
    var1=value
    echo 'this is $var1' >/tmp/f.txt
    f=$(cat /tmp/f.txt)
    echo "${f}"  # this is $var1
    echo "${[[email protected]](https://raimonster.com/cdn-cgi/l/email-protection)}"  # this is value
    

### 4.20 pass commands around

This one uses DRY_RUN. While refactoring a script that does some curls, we want to make sure that our refactored version does the exact same calls in the same order. 
    
    
    compare_outputs() {
      export DRY_RUN=1
      git checkout b1
      $@ 2>/tmp/1.out
      git checkout b2
      $@ 2>/tmp/2.out
      echo "diffing"
      diff /tmp/1.out /tmp/2.out
    }
    compare_outputs ./release.sh -p rhel:6 -R 'internal-preview'
    

First we create a function `compare_outputs`, that gets a command to run as parameters. The function will run it once, redirecting the standard error to a file `/tmp/1.out`. 

Then, it checks out the branch that contains our refactored version, and will run the command again, redirecting standard error to `/tmp/2.out`, and will diff the two outputs. 

In case there's a difference between the two, `diff` will output them, and the function will return the non-zero exit value of diff. If everything went fine, `compare_outputs` will succeed. 

Now that we know that for these inputs the command runs fine, we want to find out if it works for other types of releases, not only internal-preview. 

Here I'm using zsh's global aliases to give a much more fluid interface to the commands, but you can use the regular while/for loops: 
    
    
    alias -g SPLIT='| tr " " "\n" '
    alias -g FORI='| while read i ; do '
    alias -g IROF='; done '
    
    set -e
    echo "ga internal-preview rc1 rc2" SPLIT FORI
       noglob compare_outputs ./release.sh -p rhel:8 -R "$i"
    IROF
    

So, combining the two, we can have a really smooth way of iterating over the possibilities, without really messing into the details of loops. 

WARNING: This approach is not robust enough to put it anywhere in production, but to write quick one off scripts is a killer. Experimenting in a shell and creating tools and 2nd order tools to make interaction faster builds a language that grows on you, and keeps improving your toolbelt. 

### 4.21 The Toplevel Is Hopeless1

Shellscripts are thought as quick one-off programs, but when they are useful, they are sticky, so you better write them from the start as if it would be permanent. The upfront cost is very low anyway. Structure the script like a regular app. 

Bash is extremely permissive in what it allows to be coded and ran. By default, failures do not make the program exit or throw an exeption (no exceptions here). And for some reason, the common usage of shellscripts is to put everything in the top level. Don't do that. Do the least possible things in the toplevel. 

A way to improve the defaults, is setting a bunch of flags that make the script stricter, so it fails on many situations you'd want to stop anyway because something went wrong. 
    
    
    #!/usr/bin/env bash
    set -eEuo pipefail
    shopt -s inherit_errexit
    
    main() {
      parse_args
      validate_args
      do_things
      cleanup
    }
    
    main "[[email protected]](https://raimonster.com/cdn-cgi/l/email-protection)"
    

Ref: <https://dougrichardson.us/2018/08/03/fail-fast-bash-scripting.html>

### 4.22 Check your deps

Giving useful information to the user will only help them using the script, and you debugging it. This is a common use case, so let's do it nicely. 
    
    
    deps() {
      for dep in "[[email protected]](https://raimonster.com/cdn-cgi/l/email-protection)"; do
        mute which "$dep" || die "$dep dependency missing"
      done
    }
    
    main() {
      deps jq curl
      # ...
    }
    

### 4.23 source files

`source` is like `require` or `import` in some programming languages. It evaluates the sourced file in the context of the current script, so you get all definitions in your environment. 

It's simple, but it helps you get used to modularize your code into libraries. 

Be careful, it's very rudimentary, and it will be overwriting old vars or functions if names clash. There's no namespacing happening there. 
    
    
    source file.sh
    
    # the same
    . file.sh
    

### 4.24 Use Scripts as a Libs

A python-inspired way of using scripts as loadable libraries is to check whether the current file was the one that was called originally or it's being just sourced. 

Again, no side effects in load time makes this functionality possible. otherwise, you're on your own. 
    
    
    # Allow sourcing of this script
    if [[ $(basename "$(realpath "$0")") == "${BASH_SOURCE}" ]]; then
      setup
      parse_args "[[email protected]](https://raimonster.com/cdn-cgi/l/email-protection)"
      main
    fi
    

### 4.25 Tmpfiles Everywhere

Your script is not going to run alone. Don't assume paths are fixed or known. 

CI/CD Pipelines run many jobs in the same node and files can start clashing. 

Make use of `$(mktemp -d /tmp/foo-bar.XXXXX)`. If you have to patch a file, do it in a clean fresh copy. Don't modify files in old paths 

If you HAVE TO modify paths, do it idempotently. But really, don't do it. 
    
    
    git_clone_tmp() {
      local repo=${1:?repo is required}
      local ref=${2:?ref is required}
      tmpath=$(mktemp -d "/tmp/cloned-$repo-XXXXX")
      on_exit "rm -rf $tmpath"
      git clone -b ${ref} $repo $tmpath
    }
    

CAVEAT: You have to manually delete the directory if you want it cleaned. 

### 4.26 Cleanup tasks with trap

`trap` is used to 'subscribe' a callback when something happens. Many times it's used on exit. It's a good thing to cleanup tmpdirs after your script exits, so you can use the output of `mktemp -d` and subscribe a cleanup function for it. 
    
    
    on_exit() {
      rm -rf $1
    }
    local tmpath=$(mktemp -d /tmp/foo-bar.XXXXX)
    trap "on_exit $tmpath" EXIT SIGINT
    

### 4.27 array of callbacks on_exit

Level up that pattern, we can have a helper to add callbacks to run on exit. Get used to these kind of patterns, they are super powerful and save you lots of manual bookkeeping. 
    
    
    ON_EXIT=()
    EXIT_RES=
    
    function on_exit_fn {
      EXIT_RES=$?
      for cb in "${ON_EXIT[@]}"; do $cb || true; done
      return $EXIT_RES
    }
    
    trap on_exit_fn EXIT SIGINT
    
    function on_exit {
      ON_EXIT+=("[[email protected]](https://raimonster.com/cdn-cgi/l/email-protection)")
    }
    
    local v_id=$(docker volume create)
    on_exit "docker volume rm $v_id"
    # Use your v_id knowing that it'll be available during your script but
    # will be cleaned up before exiting.
    

### 4.28 stacktrace on error

Here's a nice helper for debugging errors in bash. In case of non-0 exit, it prints a stacktrace. 
    
    
    set -Eeuo pipefail
    trap stacktrace EXIT
    stacktrace() {
        rc="$?"
        if [ $rc != 0 ]; then
            printf '\nThe command "%s" triggerd a stacktrace:\n' "$BASH_COMMAND"
            for i in $(seq 1 $((${#FUNCNAME[@]} - 2))); do
              j=$((i+1));
              printf '\t%s: %s() called in %s:%s\n' "${BASH_SOURCE[$i]}" "${FUNCNAME[$i]}" "${BASH_SOURCE[$j]}" "${BASH_LINENO[$i]}";
            done
        fi
    }
    
    

ref: <https://news.ycombinator.com/item?id=26644110>

### 4.29 Dots and colons allowed in function names!

A way to split the namespace is to have libs define functions with their own namespace. 

I've gotten used to use dots or colons as namespace separator. 
    
    
    semver.greater() {
     # ...
    }
    

or 
    
    
    semver:greater() {
     # ...
    }
    

### 4.30 make steps of the process as composable as possible by using "[[email protected]](https://raimonster.com/cdn-cgi/l/email-protection)"

By using `[[email protected]](https://raimonster.com/cdn-cgi/l/email-protection)` to pass commands as parameters around you can get to a degree of composability that allows for a nice chaining of commands. 

here's a very simple version of `watch`. See how you can `every 2 ls -la`. I think that style is called Bernstein Chaining. But I'm not sure if it's exactly the same. It also looks like currying or partial evaluation to me if you squint a little bit. 
    
    
    every() {
       secs=$1
       shift
       while true; do
         "[[email protected]](https://raimonster.com/cdn-cgi/l/email-protection)"
         sleep $secs;
       done
     }
    

As you know by now, bash doesn't pass blocks of code around, but the alternative is to pass function names. 
    
    
    mute() {
      $@ >/dev/null 2>/dev/null
    }
    mute ls
    

So now we can create the most stupid command composition ever: 
    
    
    every 1 mute echo hi
    #or
    mute every 1 echo hi
    

For the particular redirection problem, another option is to use aliases. Redirects can be written anywhere on your CLI (not just at the end), so the following will work using a plain alias: 
    
    
    alias mute='>/dev/null 2>/dev/null'
    mute ls
    

  * <https://www.oilshell.org/blog/2017/01/13.html>



### 4.31 do_times/foreach_*

shellscripts are highly side-efffecty, and even though the scoping of variables is not very empowering, you can get a limited amount of decomposition of loops by passing function names. 

This is a lame example, but I hope it shows the use case, it allows you to group already existing functions while taking advantage of a fixed looping iterator, and leaving traces of the current loop vars in the global "variable" environment. 
    
    
    create_user() {
      uname="u$1" # leave uname in the global env so later functions see it
      http :8080/users name="$uname"
    }
    
    create_pet() {
      pname="p$1"
      http :8080/users/$uname/pets name="$pname"
    }
    
    create_bundle() {
      create_user $1
      create_pet $1
    }
    
    do_times() {
      local n=$1; shift
      for i in $(seq $n); do
        "[[email protected]](https://raimonster.com/cdn-cgi/l/email-protection)" $i
      done
    }
    
    do_times 15 create_bundle
    

A bit more complex is runnning a command to every repo in an org: 
    
    
    run_tests() {
      ./ci/test.sh
    }
    
    foreach_repo_with_index() {
      local counter=0
      local repos=$(http https://api.github.com/users/$1/repos)
      shift
      for entry in $(echo $repos | jq -r '.[].git_url'); do
        (git_clone_tmp $entry master
         cd $tmpath
         "[[email protected]](https://raimonster.com/cdn-cgi/l/email-protection)" $counter $entry
        )
        ((counter=counter+1))
      done
    }
    
    foreach_repo_with_index kidd run_tests
    

### 4.32 <(foo) and >(foo)

Some commands ask for files as inputs. And sometimes you have that file, but sometimes you're only creating that file to pass it to the command. In those cases, creating temporary files is not necessary if you use `<(cmd)`. Here's a way to diff the output of 2 commands without putting them in a temporary file. 
    
    
    diff <(date) <(date)
    diff <(date) <(sleep 1; date)
    

The same happens with outputs. Commands that ask you for a destination file. You can trick them by using `>(command)` as a file. A nice trick is to use `>(cat)` to know what's going on there. Also useful to send stuff to the clipboard `>(xclip)` before running something on the output. 

What the shell does in those cases is to bind a file descriptor of the process created inside `< or >` to the first process. 

You can experiment with those using commands like `echo <(pwd)`. 

In Zsh you can use `m-x expand-word` to see the file descriptors being expanded. 

A way to peek into a huge pipe is to `tee >(cat)`

### 4.33 Use xargs

Continuing with other ways of plumbing commands into other commands, there's `xargs`. Some commands work seamlessly with pipes, by taking inputs from stdin and printing to stdout. But some others like to work with files, and they ask for their parameters in their args list. For example, `evince`. It wouldn't be even expected to cat a pdf and pass it to evince through stdin. 

In general, to convert from this pattern: `cmd param` to `echo param| cmd`, xargs can be helpful. Look at its man page to know how to split or batch args in multiple `cmd` calls. 

Xargs is helpful for parallelizing work. You should look at its man page, but just know it can help in running parallel processes (check `-P` in its man). 

### 4.34 change loops for "mapping/reducing" functions

#### 4.34.1 find

Many times we want to run the same operation or test to lots of files. Instead of looping for each file, think if `find -exec` would solve it. Also, find supports multiple directories. 
    
    
    dirs=("/usr/local/bin" "/usr/bin")
    for d in "${dirs[@]}"; do
      for f in $(find "$d"); do
        echo "check if owner of $f is johndoe and group is johndoe"
        [ `stat -c %U:%G $f` == "johndoe:johndoe" ] || die "error"
      done
    done
    

Compare it to: 
    
    
    [ $(find "$dirs[@]" -exec stat -c '%U:%G' {} \; | grep -vc "johndoe:johndoe") == "0" ] || die "error"
    

Other examples might be: 
    
    
    # count all lines of all docx in this dir
    find . -type f -name "*docx" -exec pandoc "{}" -t plain \; | wc -l
    
    #All your files have the same owner and group permissions
    [ $(find "$files[@]" -exec stat -c '%a' {} \; | grep -Evc "^(.)\1") == "0" ]
    

#### 4.34.2 grep -Fvf

Three magical flags that go well together. 

`-f` list of patterns to match as a file. `-F` interpret the "pattern" as a Fixed string, not a pattern/regex `-v` negate the output. Print non matching lines. 

The cool thing about combining `-f` and `-v` is that the negative matches mean "lines that are not ANY of the ones in the pattern list". So you can do list diffing. like `sort + diff` but more flexible. 

Here's a practical case of finding version numbers that we have a tag for, that do not have a title in the readme 
    
    
    f="readme.md"
    ! grep -Fvf <(grep -P "^# \d\.\d\.\d\.\d$" "$f" | sed -e 's/^# //') \
                <(git tag | grep -P "^\d\.\d\.\d\.\d$")
    

### 4.35 pass flags as a splatted array

There's quite a bit to chew on this example. First of all, the core pattern is to build up your commandline options with an array, and splat it in the final command line. For complex commands like `docker` where you easily have 10+ flags it's a visual aid, and also opens up the opportunities for reusing or abstracting sets of options to logical blocks. 

Once it's an array, we can add elements conditionally to that array depending on the current run, and build the line that we'll be running in the end. 
    
    
    # Allows Ctrl-C'ing on interactive shells
    INTERACTIVE=
    if [[ -t 1 ]]; then INTERACTIVE="-it"; fi
    
    local flags=(
      # We mount it as read-only, so we make sure we are not writing anything
      # in there, and that everything is explicitly defined
      "-v $LOCAL_PATH/build-dir:/build-dir:ro,delegated"
      "-v $OUTPUT_DIR:/output:rw,consistent"
      "-v $tmp_dir:/tmp/work:rw,delegated"
    )
    if [[ -n $LOCAL_PATH ]]; then
      flags+=("-v $(realpath $LOCAL_PATH)/overrides/my-other-file:/build-dir/build.json:ro")
      flags+=("-e LOCAL_PATH=/tmp/local")
    fi
    
    local v_id=$(docker volume create)
    flags+=("-v $v_id:/tmp/build")
    on_exit "docker volume rm $v_id"
    
    docker run --rm $INTERACTIVE ${flags[*]} $image touch /tmp/build/foo.txt
    
    docker run --rm $INTERACTIVE ${flags[*]} fpm:latest fpm-build /tmp/build/foo.txt
    on_exit "chown_cache $tmp_dir"
    

In this example we see another cool trick. Mounting a volume in 2 differrent containers, so not for the purpose of sharing a local file/dir with the host but to share it between themselves. In that case, the 2 containers don't even coexist temporarily, but use the volume as a conveyor belt, passing it from container to container, and each one applies "its thing". 

After all the mess, someone has to cleanup everything, but we know how to do it with `on_exit` trick. 

### 4.36 inherit_errexit

bash 4.4+ , you can `shopt -s inherit_errexit`, and subshells will inherit the errexit flag value. meaning that if you `set -Ee`, anything that runs inside a subshell will throw an error at the moment any command exits with `!=0`. 

### 4.37 GNU Parallel

I can't recommend [parallel](https://www.gnu.org/software/parallel/) enough. The same as xargs, but in a much more flexible way, parallel lets you run various jobs at a time. If you have this tool into account, it doesn't just speed up your runtimes, but it will force you write cleaner code. Parallel execution will test your scripts so if they are not using randomized tmp working directories, things will clash, etc… 

Parallel in itself is such a hackerfriendly tool it deserves to be deeply learned. You can use it just locally to run a process per core, you can send jobs to several machines connected via a simple ssh, you can bind tmux or sqlite to it, or you can write a trivial job queuing system. 

Man pages and official examples are a goldmine. 

### 4.38 HEREDOCS

  * Basic usage of heredocs:


    
    
         echo <<EOF
    $interpolated
    \$non_interpolated
    EOF
    

  * A dash after `<<` replaces trailing spaces in here docs


    
    
    echo <<-EOF
    $var
    there
    EOF
    

  * quoting the identifier disables interpolation of variables


    
    
    echo <<'EOF'
    $non_interpolated
    there
    EOF
    

The [bash manual](https://www.gnu.org/savannah-checkouts/gnu/bash/manual/bash.html#Here-Documents) is super concise and to the point there. 

## 5 Interactive

### 5.1 Save your small scripts

Rome wasn't built in a day, and like having a journal log, most of the little scripts you create, once you have enough discipline will be useful for some other cases, and your functions will be reusable. 

Save your scripts into files early on, instead of crunching everything in the repl. learn how to use a decent editor that shortens the feedback cycle as much as possible. 

### 5.2 Increased Interactivity

Knowing your shell's shortcuts for interactive use is a must. The same way you learned to touchtype and you learned your editor, you should learn all the shortcuts for your shell. Here's some of them. 

key | action  
---|---  
Ctrl-r | reverse-history-search  
C-a | beginning-of-line  
C-e | end-of-line  
C-w | delete-word-backwards  
C-k | kill-line (from point to eol)  
C-y | paste last killed thing  
A-y | previous killed thing (after a c-y)  
C-p | previous-line  
C-n | next-line  
A-. | insert last agument  
A-/ | dabbrev-expand  
  
A written form of `A-.` is `$_`. It retains the last argment and puts it in $_. `test -f "FILE" && source "$_"`. 

### 5.3 Aliases

Aliases are very simple substitutions of commands for a sequence of other commands. Usual example is 
    
    
    alias ls='ls --auto-color'
    

Now let's move on to the interesting stuff. 

### 5.4 functions can generate aliases

Aliases live in a global namespace for the shell, so no matter where you define them, they take effect globally, possibly overwriting older aliases with the same name. 

Well, it's not lexical scope (far from it), but using aliases you can create a string that snapshots the value you want, and capture it to run it later. 

Some fun stuff: 

  * aliasgen. Create an alias for each directory in ~/workspace/. This is superceeded by `CDPATH`, but the trick is still cool.


    
    
    aliasgen() {
      for i in ~/workspace/*(/) ; do
          DIR=$(basename $i) ;
           alias $DIR="cd ~/workspace/$i";
      done
    }
    aliasgen
    

  * a make a shortcut to the current directory.


    
    
    function a() { alias $1=cd\ $PWD; }
    
    mkdir -p /tmp/fing-longer
    cd /tmp/fing-longer
    a fl
    cd /
    fl
    echo $PWD   # /tmp/fing-longer
    

A man can dream… 

  * unhist. functions can create aliases, and functions can receive functions as parameters (as a string (function name)), so we can combine them to advise existing functions. 
    
        unhist () {
      alias $1=" $1"
    }
    unhist unhist
    unhist grep
    unhist rg
    
    noglobber() {
        alias $1="noglob $1"
    }
    noglobber http
    noglobber curl
    noglobber git
    
    

  * Problem: These commands do not compose. Combination of 2 of those doesn't work, because the second acts just on the textual representation that it received, not the current value of the alias.



### 5.5 Override (advise?) common functions

Overriding commands is generally a bad practice as it violates the principle of least surprise, but there might be occasions (mostly in your local machine) where you can integrate awesome finetunnings to your toolbelt. 

Here we're going to get the original docker binary file location. After that we declare a function called `docker` that will proxy the parameters to the original `docker` program UNLESS you're calling `docker run`. In that case, we're injecting a mouted volume that mounts `/root/.bash_history` of the container to a file hosted in the host (duh). That's a pretty cool way of keeping a history of your recent commands in your containers, no matter how many times you start and kill them. 
    
    
    DOCKER_ORIG=$(which docker)
    docker () {
        if [[ $1 == "run" ]]; then
            shift
            $DOCKER_ORIG run -v $HOME/.shared_bash_history:/root/.bash_history "[[email protected]](https://raimonster.com/cdn-cgi/l/email-protection)"
        else
            $DOCKER_ORIG "[[email protected]](https://raimonster.com/cdn-cgi/l/email-protection)"
        fi
    }
    

I'm particularly fond of this trick, as it saved me tons of typing. But at a personal level, it was mindblowing that sharing this around the internet caused the most disparity of opinions. Also, I recently read the greate book "Docker in Practice" by [Ian Miell](https://github.com/ianmiell) and there's a snippet that is 99.9% like the one I created myself. That was a very cool moment. 

### 5.6 Faster iteration on pipes

When testing complex pipelines: 

  * Make them pure (no side effects).
  * One command per line.
  * End lines with the pipe character.
  * During development, end the pipeline with `cat`.



I usually use `watch -n1 'code.sh'` in a split window so I see the results of what I'm doing. The advantage of 
    
    
    curl https://www.example.com/videos/              |
      pup 'figure.mg > a attr{href}'                  |
      head -1                                         |
      xargs -I{} curl -L "https://www.example.com/{}" |
      pup 'script'                                    |
      grep file:                                      |
      sed -e "s/.*\(http[^ \"']*\).*/\1/"             |
    # xargs vlc                                       |
      cat
    

Over 
    
    
    curl https://www.example.com/videos/                \
      | pup 'figure.mg > a attr{href}'                  \
      | head -1                                         \
      | xargs -I{} curl -L "https://www.example.com/{}" \
      | pup 'script'                                    \
      | grep file:                                      \
      | sed -e "s/.*\(http[^ \"']*\).*/\1/"             \
    # xargs vlc   # doesn't work
    

Is that you can comment out lines on the former one, but you can't do that on the latter. The `cat` trick makes it so that you have an 'exit' point, and you don't have to comment that one. Also, some editors will indent the first one correctly, while you'll have to manually indent the second one. 

Small wins that compose just fine :) 

### 5.7 Use ${var?error msg} on templates

If you write something to be copypasted by your user and filled in, instead of `<var>`, try `${var?You need to set var}`. it allows for the user to set the variable in the environment without having to replace inline, and if the user forgets any, the shell will barf. 

## 6 Debugging

### 6.1 adding `bash` to a script to debug

You can add `bash` inside any script, and it'll add a sort of a breakpoint, allowing you to check the state of the env and manually call functions around. 

If you orgainse your code in small functions, it's easy to add breakpoints by just spawning bash processes inside your script. 

This works also inside docker containers (if you provide `-ti` flag on run). 

Let's see some usual uses of docker and how we can debug our scripts there: 
    
    
    # leaves you at a shell to fiddle if all is in place after build
    docker run -it mycomplex-image bash
    
    # Runs /tmp/file.sh from the host inside. That's cool to make the
    # container less hermetic. Even if the image is not originally ment
    # to, you can even override it and 'monkeypatch' the file with the one
    # from the host anyway.
    docker run -it -v $PWD:/tmp/ mycomplex-image /tmp/file.sh
    
    # So now you can really add wtv you want there.
    echo 'bash' >>$PWD/file.sh
    
    # run+open shell at runtime to inspect the state of the script
    docker run -it -v $PWD:/tmp/ mycomplex-image /tmp/file.sh
    

### 6.2 DRY_RUN
    
    
    if [[-n "$DRY_RUN" ]]; then
      curl () {
        echo curl "[[email protected]](https://raimonster.com/cdn-cgi/l/email-protection)"
      }
    fi
    

use `command curl` to force the command, not the alias or anything 

### 6.3 Cheap debugging flag
    
    
    optargs "V" option; do
    case $option in
      V)
        set -xa
      ;;
    

### 6.4 explore a pipe with tee >(some_command) |

the `>()` is not very easy to use. Very few places where it fits. Here's a nice pipe inspector though, using `tee >(cat 1>&2)` trick. 
    
    
    plog() {
      # tee >(cat 1>&2)
      local msg=${1:-plog}
      tee >(sed -e "s/^/[$msg] /" 1>&2)
    }
    alias -g 'PL'=' |plog ' #zsh only
    
    echo "a\nb" PL foo | tr 'a-z' 'A-Z' PL bar
    # output:
    # [foo] a     # stderr
    # [foo] b     # stderr
    # A           # stdout
    # B           # stdout
    # [bar] A     # stderr
    # [bar] b     # stderr
    

ref: <https://stackoverflow.com/questions/17983777/shell-pipe-to-multiple-commands-in-a-file>

### 6.5 tee+sudo

If you wat to store a file in a root-owned dir, in the middle of your pipeline, instead of running the whole thing as root, you can use `sudo tee file`: 
    
    
    ls | grep m >/usr/local/garbage  # fail
    ls | grep m | sudo tee /usr/local/garbage # success!
    

### 6.6 quoting

Bash: To get a quoted version of a given string, here's what you can do: 
    
    
    # this is my "string" I want to 'comment "on"'
    !:q
    

That gives us `'#this is my "string" I want to '\''comment "on"'\'''`. Neat! 

I just found this trick [here](https://til.simonwillison.net/til/til/bash_escaping-a-string.md). From the associated HN thread: 
    
    
    function bashquote () {
      printf '%q' "$(cat)"
      echo
    }
    

Zsh: If you're on zsh, `a-'` quotes the current line. 

## 7 zsh-only

### 7.1 Word spliting

Word splitting works differently by default in zsh than in bash. 
    
    
    foo="ls -las"
    $foo
    

This works in bash, but zsh will not split by words. To make zsh expand by words, there are 2 ways: `setopt SH_WORD_SPLIT` and `${=foo}`. zsh has `unsetop` command, which allows to scope where you want the expansions to happen. `unsetop SH_WORD_SPLIT`. 

The problem with both solutions is that none of them are compatible with bash, so you'll be cornering yourself to "this only works in zsh". A way to overcome this is to use arrays, which are expanded in the same way in both shells. 

Or, use the same hack as you'll see later with noglob. 

Refs: 

  * <https://stackoverflow.com/questions/6715388/variable-expansion-is-different-in-zsh-from-that-in-bash>
  * <http://zsh.sourceforge.net/FAQ/zshfaq03.html#l18>



### 7.2 globbing

In zsh, getting a list of files that match some characteristics is doable using globbing. Bash has globbing also, but in a less sophisticated way. 

The basic structure of a `glob` is `pattern(qualifiers)`. Patterns can contain: 

  * strings: they do exact match
  * wildcards: `*`, `?`, `**/`
  * character classes: `[0-9]`
  * choices: `(.pdf|.djvu)`



The qualifiers are extra constraints you put on the matches. There are lots of different qualifiers. Look at `zshexpn` for the complete list. The ones I use more are: 

  * `.` Files
  * `/` Directories
  * `om[numberhere]`. Nth latest modified



### 7.3 Some global aliases:

These are some aliases I have in my ~/.zshrc that somehow help me use a shell in a more fluid way. 
    
    
    alias -g P1='| awk "{print \$1}"'
    alias -g P2='| awk "{print \$2}"'
    alias -g P3='| awk "{print \$3}"'
    alias -g P4='| awk "{print \$4}"'
    alias -g P5='| awk "{print \$5}"'
    alias -g P6='| awk "{print \$6}"'
    alias -g PL='| awk "{print \$NF}"'
    alias -g PN='| awk "{print \$NF}"'
    alias -g HL='| head -20'
    alias -g H='| head '
    alias -g H1='| head -1'
    alias -g TL='| tail -20'
    alias -g T='| tail '
    alias -g T1='T -1'
    #alias -g tr='-ltr'
    alias -g X='| xclip  '
    alias -g TB='| nc termbin.com 9999 '
    alias -g L='| less -R '
    alias -g LR='| less -r '
    alias -g G='| grep '
    alias -g GI='| grep -i '
    alias -g GG=' 2>&1 | grep '
    alias -g GGI=' 2>&1 | grep -i '
    alias -g GV='| grep -v '
    alias -g V='| grep -v '
    alias -g TAC='| tac '
    alias -g DU='du -B1'
    
    alias -g E2O=' 2>&1 '
    alias -g NE=' 2>/dev/null '
    alias -g NO=' >/dev/null '
    
    alias -g WC='| wc -l '
    
    alias -g J='| noglob jq'
    alias -g JQ='| noglob jq'
    alias -g jq='noglob jq'
    alias -g JL='| noglob jq -C . | less -R '
    alias -g JQL='| noglob jq -C . | less -R '
    alias -g XMEL='| xmlstarlet el'
    alias -g XML='| xmlstarlet sel -t -v '
    
    alias -g LYNX="| lynx -dump -stdin "
    alias -g H2T="| html2text "
    alias -g TRIM="| xargs "
    alias -g XA='| xargs -d"\n" '
    alias -g XE="| xargs e"
    alias -g P="| pick "
    alias -g PP="| percol | xargs "
    alias -g W5="watch -n5 "
    alias -g W1="watch -n1 "
    
    
    alias -g CB="| col -b "
    alias -g NC="| col -b "
    alias -g U='| uniq '
    alias -g XT='urxvt -e '
    alias -g DM='| dmenu '
    alias -g DMV='| dmenu -i -l 20 '
    
    alias -g ...='../..'
    alias -g ....='../../..'
    alias -g .....='../../../..'
    
    alias -g l10='*(om[1,10])'
    alias -g l20='*(om[1,20])'
    alias -g l5='*(om[1,5])'
    alias -g l='*(om[1])'
    alias -g '**.'='**/*(.)'
    alias -g lpdf='*.pdf(om[1])'
    alias -g lpng='*.png(om[1])'
    
    alias -g u='*(om[1])'
    
    alias lsmov='ls *.(mp4|mpg|mpeg|avi|mkv)'
    alias lspdf='ls *.(pdf|djvu)'
    alias lsmp3='ls *.mp3'
    alias lspng='ls *.png'
    

Now, some sequences of words can start making sense: 

  * `lspdf -tr TL DM XA evince`
  * `docker exec -u root -ti $(docker ps -q H1) bash`
  * `docker ps DM P1 XA docker stop`



### 7.4 Expansion of global aliases

Let's dig deeper. 
    
    
    alias -g DOCK='docker ps 2..N P P1'
    DOCK  # works fine. Echoes the id of the chosen container
    docker stop DOCK   # does not work , because it expands to docker stop docker ps 2..N P P1
    docker stop $(DOCK) # works fine again
    alias -g DOCK='$(docker ps 2..N P P1)'
    docker stop DOCK   # yay!
    

So, you can bind words to expansion-time results of the aliases. It feels like a very powerful thing, to have this "compile time" expansions. Reminds me of CL's symbol-macrolet, or IMMEDIATE Forth words. 

### 7.5 Autocomplete

Writting smart autocompletion scripts is not easy. 

zsh supports `compdef _gnu_generic` type of completion, which gets you very far with 0 effort. 

When autocompleting after a `-` in the commandline, if your command is configured like `compdef _gnu_generic mycommand`, zsh will call the script with `--help` and parse the output, trying to find flags, and will use them as suggestions. It's really great. 

The compromise is to write a decent "–help" for your script. Which is cool because your user will love it too, and you just have to write it once. 

The completion is not context aware though, so you can't autocomplete flags after the first non-flag argument. It seems this could be improved in zsh-land, by asking for the –help like `mycommand args-so-far --help`. But it doesn't work like that. 
    
    
    #!/usr/bin/env bash
    # The script can be bash-only, while the completion work in zsh-only
    set -Eeuo pipefail
    
    help() {
      echo "  -h,--help    Show help"
      echo "  -c,--command Another thing"
    }
    
    if [ "$1" == "--help" ];
      help
    fi
    

Now you can play with `mycommand -<TAB>`. Amazing, wow. 

### 7.6 Create helpers and generate global aliases automagically

Borrowing a bit from Perl, a bit from Forth, and a bit from PicoLisp, I've come to create a few helpers that abstract words into a bit higher level concepts. Unifying the option selectors is one, and then, other line oriented operations like `chomp, from, till`. 
    
    
    pick() {
      if [ -z "$DISPLAY" ]; then
        percol || fzf || slmenu -i -l 20
      else
        dmenu -i -l 20
      fi
    }
    alias -g P='| pick'
    
    globalias() {
      alias -g `echo -n $1 | tr '[a-z]' '[A-Z]'`=" | $1 "
    }
    
    globalias fzf
    
    # uniquify column
    function uc () {
      awk -F" " "!_[\$$1]++"
    }
    globalias uc
    
    function from() { perl -pe "s|.*?$1|\1|" }
    globalias from
    
    function till() { sed -e "s|$1.*|$1|" }
    globalias till
    
    function chomp () { sed -e "s|.$||" }
    globalias chomp
    

Again, it's a pity those do not compose well. Just be well organized, or build a more elaborate hack so you can compose aliases with some sort of confidence. It'll always be a hack though. 

### 7.7 suffix aliases don't have to match a filename

zsh has another type of aliases called "suffix alias". Those alias allow you to define programs to open/run file types. 
    
    
    alias -s docx="libreoffice"
    

With this said, if you write a name of a file ending with `docx` as the first token in a command line, it will use libreoffice to open it. 
    
    
    invoice1.docx
    # will effectively call libreoffice invoice1.docx
    

The trick here is that the parser doesn't check that the file is indeed an existing file. It can be any string. 

Let's look at an example of it. 
    
    
    alias -s git="git clone"
    

In this case, we can easily copy a `[[email protected]](https://raimonster.com/cdn-cgi/l/email-protection):.....git` from a browser, and paste it into a zsh console. Then, zsh will run that "file" with the command `git clone`, effectively cloning that repository. 

Cool, ain't it? 

### 7.8 noglob

zsh has more aggressive parameter expansion, to the level that `[,],...` have special meanings, and will be interpreted and expanded before calling the final commands in your shell. 

There are commands that you don't want ever expanded , for example, when using `curl`, it's much more likely that an open bracket will be ment to be there verbatim rather than expanded. 

Zsh provides a command to quote the following expansions. And it's called noglob. 
    
    
    noglob curl http://example.com\&a[]=1
    

### 7.9 make noglob 'transparent' to bash

zsh and bash are mostly compatible, but there's a few things not supported in bash. `noglob` is one of them. To build a cushion inbetween, an easy way is to just create a `~/bin/noglob` file 
    
    
    $*
    

### 7.10 glob nested expansion

In <https://news.ycombinator.com/item?id=26175894> there's a nice advanced example: 
    
    
    Variable expansion syntax, glob qualifiers, and history modifiers can
    be combined/nested quite nicely. For example, this outputs all the
    commands available from $PATH: `echo
    ${~${path/%/\/*(*N:t)}}`. `${~foo}` is to enable glob expansion on the
    result of foo. `${foo/%/bar}` substitutes the end of the result of foo
    to "bar" (i.e. it appends it); when foo is an array, it does it for
    each element. In `/*(*N:t)`, we're adding the slash and star to the
    paths from `$path`, then the parentheses are glob qualifiers. `*`
    inside means only match the executables, `N` is to activate NULL_GLOB
    for the match so that we don't get errors for globs that didn't match
    anything, `:t` is a history mod used for globs that returns just the
    "tail" of the result, i.e. the basename. IIRC, bash can't even nest
    multiple parameter expansions; you need to save each step separately.
    

### 7.11 Some extra shortcuts for nice things

  * `alt-'` quotes the current line. It's like `quotemeta`. great to help you fight double and triple quoting when writing scripts.
  * `alt-#` comment and accept. Nice way to store the current line for later.
  * `ctrl-o` kill-current-line, wait for a command, and paste.



### 7.12 =()

Zsh has `<()` and `>()` like Bash, but it also has `=()`. This varant is similar to `<()` but instead of creating a temporary pipe, it creates a temporary file. That is useful if we want to run commands that require a file instead of a pipe (most times, because it uses lseek to go through it). 

Node is an example of this. 
    
    
    node <(echo 'setTimeout(() => console.log("foo"), 400)')   # fails
    node =(echo 'setTimeout(() => console.log("foo"), 400)')   # works!
    

Or, 
    
    
    docker run --rm -ti -v =(echo "OHAI"):/tmp/foo ubuntu cat /tmp/foo
    

## 8 TODO patterns

### 8.1 just use cat/netcat/pipes with <()

  * input 

`python logger.py executable` will run the executable and monitor it for error messages. Depending on the error messages it will be doing. 

In order to test it, I want to run it with my own output. So what I do is `python logger.py cat`. That way I can type my stuff there, and even better, I can use a stream from the shell. `myexecutable | python logger.py cat` still works. 




#### 8.1.1 what's the unifying theory behind all that?

It's still not clear to me how they relate, but the feeling is that there's a common thread ruling all those commands. as if they generalize over the same things, or just a couple of very interrelated things. 

`echo` is to `cat` what `|` is to `xargs`. and `<()` and `>()` are able to make static files be dynamic streams. putting `cat` and `echo` inside `<()` seem like either a noop, or a leap in what can be done there. Still have to figure it out. 

<(grep a file.txt) , | xargs , cat, echo 

you-have\it-wants | executable | file | stream  
---|---|---|---  
executable | X | <(exe) | exe &vert  
file | <(cat file) | X | cat file &vert  
stream | cat | <(grep foo file.txt) | X  
  
  * output 

Most of those can be tested with and `tee`. Sometimes you would like the output to be an output to a file to be extramassaged. 

you-have\it-wants | executable | file | stream  
---|---|---|---  
executable | X | >() |   
file |  | X |   
stream |  | >(cat) | X  
  
lnav <(tail -F /my/logfile-that-gets-rotated-or-truncated.log) cat <(date) 




### 8.2 redirects & streams

  * <https://catonmat.net/ftp/bash-redirections-cheat-sheet.pdf>
  * <https://catonmat.net/bash-one-liners-explained-part-three>
  * <https://github.com/miguelmota/bash-streams-handbook>
  * <https://www2.dmst.aueb.gr/dds/sw/dgsh/>



### 8.3 The $0 pattern

<https://www.reddit.com/r/oilshell/comments/f6of85/four_more_posts_in_shell_the_good_parts/>

### 8.4 use git staging area to diff outputs of commands

<https://chrismorgan.info/blog/make-and-git-diff-test-harness/>

### 8.5 coprocs

  * <https://stackoverflow.com/questions/7942632/how-to-extrace-pg-backend-pid-from-postgresql-in-shell-script-and-pass-it-to-ano/8305578#8305578>
  * <https://unix.stackexchange.com/questions/86270/how-do-you-use-the-command-coproc-in-various-shells>



## 9 links

  * <https://www.gnu.org/savannah-checkouts/gnu/bash/manual/bash.html>
  * <https://www.gnu.org/software/bash/manual/html_node/>
  * <https://tldp.org/LDP/abs/html/>
  * <https://mywiki.wooledge.org/BashPitfalls>
  * [Gary Bernhardt. The Unix Chainsaw](https://www.youtube.com/watch?v=sCZJblyT_XM)
  * [[email protected]](https://github.com/spencertipping/). This guy has some really sick snippets
  * <https://news.ycombinator.com/item?id=23765123>
  * <https://medium.com/@joydeepubuntu/functional-programming-in-bash-145b6db336b7>
  * <https://www.youtube.com/watch?v=yD2ekOEP9sU>
  * <http://catern.com/posts/pipes.html>
  * <https://ebzzry.io/en/zsh-tips-1/>
  * <https://github.com/ssledz/bash-fun>
  * <https://news.ycombinator.com/item?id=24556022>
  * <https://www.datafix.com.au/BASHing/index.html>
  * <https://susam.github.io/tucl/the-unix-command-language.html>
  * <https://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html>
  * <https://www.grymoire.com/Unix/Sh.html>
  * <https://github.com/dylanaraps/pure-sh-bible>
  * <https://shatterealm.netlify.app/programming/2021_01_02_shiv_lets_build_a_vcs>
  * <https://news.ycombinator.com/item?id=24401085>
  * <https://git.sr.ht/~sircmpwn/shit>
  * [bocker](https://github.com/p8952/bocker). Docker implemented in around 100 lines of bash.
  * <https://github.com/simplenetes-io/simplenetes> wow



## 10 From shell to lisp and everything in between

  * [Oil Shell](https://github.com/oilshell/oil).
  * [Rash](https://rash-lang.org/) (Racket shell)
  * [PaSh](https://arxiv.org/pdf/2007.09436.pdf): Light-touch Data-Parallel Shell Processing.
  * [Painless emacs remote shells](https://www.eigenbahn.com/2020/07/08/painless-emacs-remote-shells). Because emacs has you covered
  * <https://news.ycombinator.com/item?id=24249646> rust
  * <https://github.com/liljencrantz/crush>
  * <https://github.com/artyom-poptsov/metabash>
  * <https://www.nushell.sh/>
  * [Babashka](https://github.com/borkdude/babashka)
  * Bash to Perl/Python/Ruby using ```` and growing from there.



## 11 Credits

  * Raimon Grau <raimonster@gmail.com>.
  * Some examples are result of Raimon's and Lluís Esquerda's conversations or real world examples.
  * people in <https://news.ycombinator.com/item?id=24402571> which I'll be pulling in as time allows.



## Footnotes: 

1

<https://gist.github.com/samth/3083053>

Author: Raimon Grau

[Emacs](https://www.gnu.org/software/emacs/) 26.1 ([Org](https://orgmode.org) mode 9.1.9)

[Validate](https://validator.w3.org/check?uri=referer)