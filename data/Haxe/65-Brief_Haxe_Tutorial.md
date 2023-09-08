---
date: 10-13-22
id: 65
path: Haxe
tags:
- haxe
title: Brief Haxe Tutorial
type: bookmark
url: http://www.unexpected-vortices.com/haxe/brief-tutorial.html
---

This is a concise, fast-paced, limited tutorial covering just enough [Haxe](https://haxe.org/) to get you productive right away.

[Haxe](https://haxe.org/) is a modern, general-purpose, high-level, statically-typed, object-oriented programming language with a familiar syntax. It can be compiled to native, to VM bytecode (its own HashLink VM, or to the JVM), to _numerous_ target programming languages, or can run code using its own built-in interpreter.

For a short sample of some Haxe code, visit [haxe.org](https://haxe.org/) and scroll down.

Itâs assumed here that youâre already familiar with some other JavaScript-/Java-/ActionScript3-style language.

In the text below where I write, â${feature} works just as youâd expect it ought toâ, thatâs intended to mean that Haxe does just what youâd think a sensible, block-structured, lexically-scoped, modern language would do. `:)`

This document covers Haxe version 4. If you donât already have it installed, and youâre on GNU/Linux, you might have a look at [my short getting-started guide](./getting-started.html).

# Language Overview

Some characteristics of Haxe:

  * statically-typed, with type inferencing
  * garbage-collected
  * block-structured, lexically-scoped
  * syntax is your garden-variety C-style (JavaScript-style) curlies and semis
  * OOP support (functions go in classes)
  * first-class functions
  * everything is an expression (ex. `if`/`else`, `switch`/`case`, etc. â more about this below)
  * consistent, sensible, modern programming language without excessive syntactic sugar
  * supports advanced features (not (yet) covered in this tutorial) such as abstracts, pattern matching, and macros



See also the [official Language Introduction](https://haxe.org/documentation/introduction/language-introduction.html).

# Pros and Cons of Haxe

## Pros

  * Haxe can target [a lot of different platforms](https://haxe.org/documentation/introduction/compiler-targets.html), including its very own [HashLink](https://hashlink.haxe.org/) VM.
  * compiled, and statically-typed, so Haxe helps you find your mistakes as soon as possible (preferably at compile-time)
  * You can use Haxe within ${other-language} projects, as long as Haxe can target that other language or platform â which it usually can.
  * You can easily make use of libraries from the language your Haxe project targets.
  * The [Haxe library repository](https://lib.haxe.org/) provides a nice catalog of community-provided Haxe libraries installable via the handy and standard `haxelib` tool.
  * active and supportive community (see [Haxe community forum](https://community.haxe.org/))
  * nice [manual](https://haxe.org/manual/introduction.html), [API docs](https://api.haxe.org/), [code cookbook](https://code.haxe.org/), [website](https://haxe.org/) (with [Haxe videos section](https://haxe.org/videos/)), and [blog](https://haxe.org/blog/)
  * good support for a handful of IDEâs via plug-ins
  * lots of support for game development, if thatâs your interest
  * free-software licensed (GPLv2+ for compiler, MIT for std lib and HashLink), community-focused, and not beholden to any particular corporate interests
  * fast and efficient executables for natively compiled targets
  * the compiler is very quick
  * probably the easiest migration path from Flash / ActionScript 3
  * [Support plans](https://haxe.org/foundation/support-plans.html) are available from the Haxe Foundation if your company desires paid support.



## Cons

  * docs could use more examples, and in some areas, more content
  * would like to see more users blogging about their use of Haxe



# Languages Comparable to Haxe

Haxe first appeared in 2006. The following are a handful languages (roughly in order by date-first-appeared) that are comparable to Haxe (that is, being statically-typed, GCâd, OO, âapplication-levelâ, modern languages with familiar syntax):

[Dart](https://dart.dev/) | 2011 | BSD | From Google  
---|---|---|---  
[Kotlin](https://kotlinlang.org/) | 2011 | Apache 2.0 | From JetBrains  
[TypeScript](http://www.typescriptlang.org/) | 2012 | Apache 2.0 | From Microsoft  
[Swift](https://swift.org/) | 2014 | Apache 2.0 | From Apple  
[ReasonML](https://reasonml.github.io/) | 2016 | MIT | From Facebook  
  
> Two observations:

# Setup for this Tutorial

This tutorial was written using Haxe 4 on GNU/Linux, utilizing `haxe`âs built-in interpreter (âinterpâ, aka âevalâ). To install Haxe, see my [getting started doc](getting-started.html).

> To get set up targetting Haxeâs own HashLink VM, see my [getting started with HashLink doc](./getting-started-hl.html). If youâd like to target some other environment, visit [the Haxe introduction](https://haxe.org/documentation/introduction/) and follow the link buttons under âSetup your development environmentâ.

Create and populate your project directory:

with src/Main.hx containing:

and interp.hxml containing:
    
    
    -p src
    -m Main
    --interp

Run your code on the command line from the project directory like so:
    
    
    haxe interp.hxml

> Note that when _not_ using interp (Haxeâs built-in interpreter) â that is, when compiling to some target platform â youâd usually have to type two separate commands:
> 
>   1. a `haxe` invocation to compile/build your program, and then
>   2. a target-specific command to run or otherwise use the output that `haxe` just produced.
> 

> 
> Using interp (aka eval), itâs just the one step though.

## hxml files

For invoking the `haxe` command, rather than type out the necessary command line options every time, you can instead put those options into a .hxml (âhax em ellâ) file. For example, instead of typing:
    
    
    haxe -p src -m Main --some-target whatev

you can instead create a target-specific some-target.hxml file containing those options,
    
    
    -p src
    -m Main
    --some-target whatev

and then type `haxe some-target.hxml` to get the same result.

If you build your project for multiple different targets, youâll likely have multiple hxml files â one for each target (ex., interp.hxml, hl.hxml, lua.hxml, etc.), or maybe one for each domain (ex., desktop.hxml, mobile.hxml, web.hxml).

hxml files can contain comments, which start with `#`.

# The Language

## Basics

Haxe is not whitespace-sensitive.

Haxe is case-sensitive. Variable names are often written camelCase, with types capitalized LikeThis.

`null` is used to indicate when a variable refers to no object.

`Void` is used to indicate the absence of type (ex. when used as the return type of a function).

## Basic Types

The so-called âbasic typesâ are Bool, Int, and Float, with literals like:

> On HashLink, Ints are 32-bit signed (so, -2,147,483,648 â 2,147,483,647, and they wrap), and Floats are 64-bit.

## Strings

Strings are immutable and Unicode. Not a âbasic typeâ, but thereâs a built-in literal syntax for strings, and string interpolation is supported:

See the [String API docs](https://api.haxe.org/String.html) for full list of String fields (ex. `length`, `charAt()`, `charCodeAt()`, `indexOf()`, `split()`, â¦).

## Variables and Constants

You use `var` or `final` to define variables. Variables are typed. Thanks to type inferencing, you donât often need to explicitly specify the type of a variable:

`final` variables canât be reassigned to another object, but they can refer to a mutable object (which could itself be modified).

On HashLink (and other statically-typed targets):

When types cannot be inferred you must include them explicitly. Type names come after the variable name and colon, for example:

To see the type of any variable, use `$type(x);` which will print out a warning line to the console at compile-time indicating `x`âs type.

## Classes

A class defines a new type, and typically goes into its own like-named file.

Classes have fields which may be variable fields or function fields (aka methods). Fields also may be member (aka, non-static, aka instance) or static. Summarizing:

**variable:** | member variable | static variable  
---|---|---  
**function:** | (member) method | static method  
  
> _Terminology:_ class fields that are functions (that is, functions belonging to a class) are referred to as âmethodsâ. In Haxe you can create local functions as well, but those arenât considered methods.

By default, all fields are private and member.

As an example, from your project directory make a src/my_pkg/MyClass.hx file:

and use it from Main.hx:

## Functions

In Haxe, functions are defined within classes (or within other functions). Youâve seen examples above.

Function args can have default values.

When necessary you can explicitly specify return type:

You return values with a `return` statement.

## Regarding Copying

## Arrays
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    // Removes `len` elements from an array, starting at position `pos`.
    
    
    // So,
    //   * add stuff with: push, unshift, and insert:
    //         "push/unshift onto", "insert into"
    //   * remove stuff with: pop,  shift, splice, and remove:
    //         "pop/shift off", "splice out of", "remove from"
    
    
    
    
    
    // Print an array:
    
    // Another way:
    Sys.println(a);
    // Another way, to see how the sausage is made:
    
    
    // Array comprehension (creates an array)
    
    
    // sort an array, in-place
    a.sort(Reflect.compare);
    // or, if you want to write your own comparison function:
    
    // or, more generally:
    a.sort(
        (a, b) -> {
    
    
    
        }
    );
    
    // Note, Haxe doesn't do array index bounds checking:
    
    
    
    
    
    
    
    

See the [arrays article of the code cookbook](https://code.haxe.org/category/beginner/arrays.html) for more examples.

See also the [Array API docs](https://api.haxe.org/Array.html).

## Maps

See the [maps article in the code cookbook](https://code.haxe.org/category/beginner/maps.html) for more examples.

See also the [Map API docs](https://api.haxe.org/Map.html).

## Sets

Haxe doesnât come with a Set type out of the box, but you can use the thx.Set type from the thx.core library, as described later in the section about using libraries from the Haxe library repo.

Usage looks like:

See the [thx.Set API docs](http://thx-lib.org/api/thx/Set.html).

## Type Parameters

If you want to create a new empty Array or Map, you can use the `new` keyword and also specify what type the Array or Map will contain. The syntax for that is:

The types written in the angle brackets are the type parameters.

## Enums

Built-in support for enums:

But they can do more than that. See:

## Anonymous Structures

If you just need a simple structure to group heterogeneous data (recall, Mapsâ values must be all of the same type), and you donât need a class, you might use an anonymous structure:

They can contain and be nested in other data structures, as youâd expect.

Donât confuse anonymous structures with maps even though they print out similarly in the terminal:

(Well, and if youâre familiar with Python: Haxe anon structs look very similar to Python maps/dicts.)

> Note that usually in Haxe when you see a `:` thereâs a type after it â not so for anon structs though. My advice: when writing types after a variable, omit any extra spaces. When writing anon structs, put that space after the colon. I believe [the Haxe code formatter](https://github.com/HaxeCheckstyle/haxe-formatter) does this as well.

To avoid repetition you can use `typedef`:

Note, that typedef cannot be declared inside a class.

For more info, see the [anon structs manual chapter](https://haxe.org/manual/types-anonymous-structure.html).

## Operators

Haxe has the usual operators youâd expect.

It also supports both pre- and post- increment/decrement, and they work like youâd expect.

Haxe supports the ternary operator (`foo ? bar : baz`), but you can often nearly as easily use `if`/`else` as an expression instead:

Division of numbers always gets you a Float.

Logical âandâ, âorâ, and ânotâ are spelled `&&`, `||`, `!`. These and also the comparison operators (`<`, `>`, etc.) result in a Bool.

`&&` and `||` short-circuit, like youâd expect.

All the usual compound assignment operators (like `+=`, `*=`, etc.) are supported.

Haxe also supports the range operator, `5 ... 8`, which gets you 5, 6, 7.

Thereâs no operator for exponents; explicitly use `Math.pow()`.

## Flow Control

Haxe supports the usual flow control statements, most (all?) of which are actually _expressions_ in Haxe:

Flow control statements that expect a Bool must get a Bool (for example, as returned by the equality and comparison operators); thereâs no notion of âtruthinessâ and âfalsinessâ like in scripting languages.

Note, since blocks evaluate to an expression, if you only have one expression in the `if` body anyway, you can do:

Switch:

Thereâs much more to [the switch statement](https://haxe.org/manual/expression-switch.html) though, since it supports pattern matching.

## Scoping

Scoping is lexical, and works just as youâd expect it ought to.

Further, you can create a block with its own local scope, and it works like youâd expect:

The value of the block is the value of its last expression.

Loop variables are scoped to inside the loop body:

## Regular Expressions

Haxe has built-in support for and literal syntax for regexes (ex. `var rx = ~/[a-f]+/;`). For more info, see:

## Exceptions

Haxe has try/catch and throw for exception handling. See [the try/catch manual chapter](https://haxe.org/manual/expression-try-catch.html) for more info.

# Libraries, Packages, and Modules

Haxe packages correspond to directories, and Haxe modules correspond to .hx files. Package names (and their corresponding directory names) are lowercase and may contain underscores. Module names (and their corresponding filenames) are capitalized and CamelCase. Modules contain types, and type names are also written CamelCase.

Take for example the file foo/bar/Baz.hx containing class Baz. The module Baz resides in package foo.bar (and at the top of Baz.hx it says `package foo.bar;`). The fully-qualified module name is foo.bar.Baz. The fully-qualified typename for class Baz is foo.bar.Baz.Baz. In general, you access the types in a module like so:
    
    
    {pkg-name}.{module-name}.{type-name}

However, for that special case where module name == type name, Haxe allows you the shortcut of writing `foo.bar.Baz` to mean `foo.bar.Baz.Baz`.

Haxe finds modules by searching its classpath (see the `--class-path|-p` compiler option), and also by looking where `--library|-L` specifies.

## Name Visibility

Importing a module allows you to use its unqualified module name (without its package name) in your code. You _could_ go ahead and use modules without importing them, but then youâd have to fully-qualify them with their package name every time you use them.

If your module contains more than one type, any types named differently than the module name are called âsub-typesâ. If you import a module containing subtypes, theyâll be available unqualified just like the type named after the module. That is to say, when you import a module, you get all of its types.

## Libraries

Libraries are archive files you download from the online Haxelib library repository using the `haxelib` tool. They contain modules residing within packages.

Alas, because many types of archive files are sometimes referred to as âpackagesâ, the term âpackageâ is sometimes casually used as a synonym for âlibraryâ.

# Using the Standard Library

Using the standard library (documented at <https://api.haxe.org/>) doesnât require any special imports or compiler arguments.

See [the Haxe code cookbook](https://code.haxe.org/) for examples using the standard libary, though hereâs a couple of choice tidbits anyway.

## Math, Random, and Parsing Numbers from Strings

## stdin, stdout, stderr

Reading from stdin and writing to stdout and stderr.

### stdin

You can read from stdin interactively from the command line. You can also pipe input to your Haxe program just as you would with any other command line utility.

To read in one line:

If you want to iteratively read in lines:

You could also read in all the input in one shot:

### stdout

Thereâs a few ways to write to stdout:

You can also use `Sys.stdout()` to grab the stdout object and call its write methods (see [haxe.io.Output](https://api.haxe.org/haxe/io/Output.html)).

### stderr

To write to stderr:

## Command Line Args

## Exiting

# Static Extension

Haxe can do a special trick, strictly for convenience: If thereâs a Type (typically not one youâve written yourself; call it, say, `SomeClass`) to which youâd like to add member functions, you can:

  1. Create a SomeStaticExtClass containing static methods which you wish SomeClass had as member functions. Those static methods should take an instance of SomeClass as their first argument.
  2. Back in your Main.hx, instead of `import some_pkg.SomeStaticExtClass`, do `using some_pkg.SomeStaticExtClass`. This makes the magic happen.
  3. Now when you make these wished-for method calls on instances of SomeClass, Haxe will behind-the-scenes translate them into calls to those static methods with the instance passed in as the first argument.



A few modules in the standard library were written with this usage in mind. See the [static extension chapter of the manual](https://haxe.org/manual/lf-static-extension.html).

# Using Libraries from Haxelib

The online repository of Haxe libraries and also the library management command line tool are both called â[haxelib](https://lib.haxe.org/)â. When you install the binary release of Haxe onto GNU/Linux, it comes with both `haxe` (the compiler) and `haxelib` (the library management tool). See also `haxelib --help`.

Libraries you install via haxelib are often (though not always) named lowercase with dashes where necessary. They are sometimes named using dots instead of dashes.

> Note that the name of a library does not necessarily have anything to do with the packages and modules it contains. For example, the âthx.coreâ library contains modules in the thx package (thereâs no âthx.coreâ package).
> 
> Unfortunately, if you name libraries with dots they tend to look like package names, which can be confusing. I think using dashes makes more sense.

A library archive file itself is a .zip file, and its haxelib.json file is the libraryâs config file.

By default, `haxelib` installs and unpacks libraries into your ~/haxelib directory. This is your local haxelib-installed libraries repo. Haxelib can also manage project-specific repos.

To use a library from the [haxelib library repository](https://lib.haxe.org/):

  1. install the library you want, ex.: `haxelib install cool-stuff`
  2. Into your .hxml build file add a line like `--library cool-stuff`
  3. In your code, you can now use that libâs fully-qualified typenames (or, if youâd rather not have to fully-qualify names, import modules from the library).



## Using the haxelib command

Examples:

Itâs often very useful to install packages directly from their git repo, rather than waiting for their lib.haxelib.org package to be updated. To do that, for example, for the thx.core library:
    
    
    haxelib git thx.core https://github.com/fponticelli/thx.core.git

(see [docs for haxelib git](https://lib.haxe.org/documentation/using-haxelib/#git))

If youâre working on a library on your own system, which youâd like to install (locally, into your ~/haxelib) straight from its project directory and without having to first publish it to lib.haxe.org, you can do:
    
    
    haxelib dev my-proj ~/path/to/my-proj

For more info, see [the haxelib docs](https://lib.haxe.org/documentation/).

## Example: CSV files

To work with csv files, try the [thx.csv](https://lib.haxe.org/p/thx.csv/) module. Install the lib:

To your .hxml file, add the line `--library thx.csv`

In your code, `import thx.csv.Csv` and use this module like so:

## Example 2: Set Type

Install `haxelib install thx.core`, and `import thx.Set`. See Sets above for usage.

## `--class-path` vs `--library`

Note the difference between the two following `haxe` compiler options:

`--class-path|-p`
    This specifies any additional directories where the compiler is to look for classes (the classpath). 
`--library|-L`
    This specifies a particular library (typically in your ~/haxelib) that you want included into your build. 

Note, you donât add the path to the Haxe standard library to your classpath â thatâs what the $HAXE_STD_PATH environment variable is for. For example, with my installation the Haxe standard library is located in ~/opt/haxe/std, and so in my ~/.bashrc I have:

# Documenting Your Library

To generate HTML API docs, use [dox](https://lib.haxe.org/p/dox/).

It works like this:

  1. document your libraries with doc comments like `/** ... */`.

  2. _to be continued_ â¦




# Recommended Libraries

## Console

For coloring output in the terminal, try [Console.hx](https://lib.haxe.org/p/Console.hx/). Super easy to use.

## Utility Libraries

todo

## Unit Testing

One popular testing library is [munit](https://lib.haxe.org/p/munit/).

See also [discussion on other options](https://community.haxe.org/t/poll-unit-test-frameworks-for-haxe/545).

# Links

Docs:

Misc:

HaxeUI (GUI toolkit):

More links:

  * See [travix](https://github.com/back2dos/travix), for automated building on multi and varied targets.
  * [hxnew](https://github.com/markknol/hxnew) for quickly creating a new empty starter project.
  * [tink_web](https://github.com/haxetink/tink_web)
  * [thx.core](https://github.com/fponticelli/thx.core)
  * [format](https://lib.haxe.org/p/format/) â read and write different file formats



# Whatâs Next?

  1. Start a fun project.
  2. Go read [the Haxe Manual](https://haxe.org/manual/introduction.html) to learn about all the more advanced stuff not covered in this tutorial!
  3. Peruse [the Haxe code examples cookbook](https://code.haxe.org/).
  4. Keep [the Haxe API docs](https://api.haxe.org/) handy.
  5. Enjoy, and see you at [the forum](https://community.haxe.org/)!



# TODO

  * externs
  * abstracts
  * metadata (`@foo` and `@:bar`)
  * more about enum, ADT, and pattern matching
  * class properties