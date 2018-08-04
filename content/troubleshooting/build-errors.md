---
title: "BUILD ERRORS"
date: 2018-01-20T12:35:19-08:00
draft: false
---

<p style="text-align: center"><em>"It is said that despite its many glaring (and occasionally fatal) inaccuracies, the Hitchhiker's Guide to the Galaxy itself has outsold the Encyclopedia Galactica because it is slightly cheaper, and because it has the words 'DON'T PANIC' in large, friendly letters on the cover."</em><br/>- <a href="https://en.wikipedia.org/wiki/Phrases_from_The_Hitchhiker%27s_Guide_to_the_Galaxy">The Hitchhiker's Guide to the Galaxy</a></p>


If you make a mistake in your code, Go will often be able to tell you when you build your program; before you even run it.  When Go does notice a mistake after you run `go build`, it will show you an error message that looks a bit like this:

{{< highlight shell >}}
$ go build maze.go 
# command-line-arguments
maze.go:15: <error goes here>
maze.go:17: <error goes here>
maze.go:21: <error goes here>
{{< /highlight >}}

This example is telling us that we have three errors, one for each error line.  Each error line tells us three bits of information:

1. Which file the error is in.  For now we only have one file so it should always say `maze.go`.
2. Which line number the error is on.  Our example is telling us that there are errors on lines 15, 17 and 21.  When we are trying to understand what the error is telling us, we should always go to the line it tells us and see what it says.
3. The actual error.  This will be different depending on what kind of mistake we made.  Look at the list below to get more help on different types of error.

{{< notice green >}}DON'T PANIC! If you have lots of errors when you run <strong>go build</strong>, always fix the <strong>top</strong> one first!  Sometimes the errors lower down the list are actually caused by the errors higher up.  Its important to fix the top one first to avoid wasting time and effort trying to understand the errors lower down.{{< /notice >}}

### Types of Error

Here's a list of common build errors we might see when we run `go build` for our program.  Follow the link to see how to fix the error.

{{< notice yellow >}}
Note: In the examples below we use <strong>foo</strong>, <strong>bar</strong> and <strong>zip</strong> as placeholder names.  In your real error messages you will see the actual names used in your program instead.
{{< /notice >}}

- [maze.go:XX: undefined: _foo_](#undefined-foo)
- [maze.go:XX: syntax error: ...](#syntax-error)
- [maze.go:XX: not enough / too many arguments in call to ...](#not-enough-too-many-arguments)
- [maze.go:XX: type _Foo_ is not an expression](#type-foo-is-not-an-expression)

### undefined _foo_

{{< highlight shell >}}
maze.go:XX: undefined: foo
{{< /highlight >}}

This error is saying that we have used _foo_ without telling Go what _foo_ is (ie. if it is a variable, function, etc).  There are lots of ways to accidentally end up with this error.  Here are some of the common ones:

- __Typo__ - A common mistake is to mistype the name of a variable or function.  Perhaps you meant to type `fop` but you typed `foo` by accident.  Go doesn't know that you meant `fop`.  It only knows that you used `foo` and didn't tell it what `foo` is.  Be careful to check capital and small letters as well.  In Go `Foo` is a different variable from `foo`.
- __= instead of :=__ - In Go you use `=` to set the value of a variable (eg. `foo = 4`).  But if you are introducing the variable `foo` for the first time, you need to use `:=` (eg. `foo := 4`).  If you forget to add the `:` the first time you use a variable, you will get the __undefined: foo__ error.
- __Missing maze.__ - Most of the functions and variable types we will use come from the `maze` package.  So they must start with `maze.`.  For example, if you forget to add `maze.` in front of the `MoveRight` function you will get __undefined: MoveRight__ because Go doesn't know that you meant the function in `maze`.

### syntax error

{{< highlight shell >}}
maze.go:XX: syntax error: blah blah blah blah
{{< /highlight >}}

A syntax error happens when you don't follow the rules for writing Go code.  Sometimes they can happen because you didn't understand the rules properly, but usually they are caused by a simple, accidental mistake.  Here are some of the common ones:

- __Missing Brackets__ - You will often get a syntax error if you forget an open or close bracket somewhere.  Also make sure you use the correct brackets - if you use a round bracket `()` when it should be a curly bracket `{}`, or the other way round, you will get a __syntax error__.
- _&& and ||_ - If you want to break up a long boolean expression over more than one line, you have to put the && (__and__) and || (__or__) symbols at the end of each line.

### not enough / too many arguments

{{< highlight shell >}}
maze.go:XX: not enough arguments in call to Foo
        have ()
        want (*maze.Maze)
maze.go:XX: too many arguments in call to Foo
        have (*maze.Maze, *maze.Maze)
        want (*maze.Maze)
{{< /highlight >}}

{{< notice blue >}}Note: Variables that we pass to a function are called <strong>arguments</strong>.  They appear in round brackets after the function name (eg. <strong>Move(m *maze.Maze)</strong> - <strong>m</strong> is an argument to the <strong>Move</strong> function).{{< /notice >}}

This error happens if we forget to pass a variable to a function when we need to, or if we accidentally pass too many.  For example if we forget to pass our maze to the `maze.MoveRight`:

{{< highlight go >}}
  maze.MoveRight() // Wrong: will cause 'not enough arguments' error
  maze.MoveRight(m) // Correct!
{{< /highlight >}}

### type _Foo_ is not an expression

This means we wrote a variable type in our code in the wrong place.  There could a lot of ways to get this error, but the most likely one is that we put a type in a function call, eg:

{{< highlight go >}}
  maze.MoveRight(m *maze.Maze) // Wrong: will cause 'type maze.Maze is not an expression' error
  maze.MoveRight(m) // Correct!
{{< /highlight >}}

