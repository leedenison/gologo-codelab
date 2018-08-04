---
title: "RUNTIME ERRORS"
date: 2018-01-20T12:35:12-08:00
draft: false
---

<p style="text-align: center"><em>"The hurrier I go, the behinder I get."</em> - <a href="https://en.wikipedia.org/wiki/Alice%27s_Adventures_in_Wonderland">Alice's Adventures In Wonderland</a></p>

There are many mistakes that Go cannot spot until we run our code.  In fact Go doesn't know what our code is supposed to do.  So there are lots of mistakes that Go will never spot for us - we just have to run our code and check it does the right thing.  Sometimes though, when our code is doing the wrong thing, our code will try to do something that is obviously a mistake.  When that happens, Go will stop the program and show us a runtime error:

{{< highlight shell >}}
$ ./maze 
panic: runtime error: integer divide by zero

goroutine 1 [running, locked to thread]:
main.DivideBy(0)
    /<directory name>/maze.go:69 +0x109
main.main()
    /<directory name>/maze.go:75 +0x11
{{< /highlight >}}

#### DON'T PANIC

{{< notice green >}}DON'T PANIC! Even though it says <strong>panic</strong>, you shouldn't panic! In Go, <strong>panic</strong> mode is just how it handles runtime errors.  For now we don't need to know exactly how panic mode works, except that it prints this message and stops the program.{{< /notice >}}

{{< highlight shell >}}
panic: runtime error: integer divide by zero
{{< /highlight >}}

The first line tells us that a runtime error happened and what it was (in this example it was a divide by zero error - noone knows what the answer should be if you divide a number by zero, so Go will `panic` with a runtime error if you try).

#### goroutine

{{< highlight shell >}}
goroutine 1 [running, locked to thread]:
{{< /highlight >}}

The next line tells us which goroutine was running when the runtime error happened.  For now, we will only be using one goroutine.  So this will always say `goroutine 1`.

#### Stack Trace

{{< highlight shell >}}
$ ./maze 
main.DivideBy(0x0)
    /<directory name>/maze.go:69 +0x109
main.main()
    /<directory name>/maze.go:75 +0x11
{{< /highlight >}}

The next bit is called a stack trace and tells us what line of code was being run when the runtime error happened.  The top two lines are the most important - they tell us exactly which function (`main.DivideBy`) and which line of our code (`maze.go:69`) was running when the error happened.  The rest of the lines tell us how we got to `maze.go` line 69 (for example, since all Go programs start with a `main` function, the last two lines of the stack trace should always be `main`).

{{< notice blue >}}
Note: Numbers that start with <strong>0x</strong> (eg. <strong>0x109</strong>) are normal numbers, but written differently to the way we are used to.  They are written in <a href="https://en.wikipedia.org/wiki/Hexadecimal">hexadecimal</a>.  We don't need to worry about how to read them for now, but if we see a number written as 0x1234, we know what it is.
{{< /notice >}}

### Types of Error

Here's a list of common runtime errors we might see when we run our program.  Follow the link to see how to fix the error.

{{< notice yellow >}}
Note: In the examples below we use <strong>foo</strong>, <strong>bar</strong> and <strong>zip</strong> as placeholder names.  In your real error messages you will see the actual names used in your program instead.
{{< /notice >}}

- [panic: invalid argument to _Foo_](#invalid-argument-to-foo)

### panic: invalid argument to _Foo_

{{< notice blue >}}Note: Variables that we pass to a function are called <strong>arguments</strong>.  They appear in round brackets after the function name (eg. <strong>Move(m *maze.Maze)</strong> - <strong>m</strong> is an argument to the <strong>Move</strong> function).{{< /notice >}}

This happens when we call a function and we give it an argument that it can't handle.  For example, if we wrote:

{{< highlight go >}}
  bar := rand.Intn(-1) // Return a random number greater than 0 and less than -1 - impossible!
{{< /highlight >}}

We could get the error `panic: invalid argument to Intn` because `rand.Intn` doesn't know how to handle an argument of -1.

Watch out when we use a variable as the function argument it is harder tell how the error happened.  For example, if we write:

{{< highlight go >}}
  bar := -1
  /* some other code ... */
  zip := rand.Intn(bar) // Go will tell us to look at this line!
{{< /highlight >}}

The error message from Go will tell us to look at the line `zip := rand.Intn(bar)`.  Just looking at that line its not easy to see why `bar` is an 'invalid argument'.  We have to look earlier in the program to see that `bar` is set to -1, which `rand.Intn` can't handle.  You have to think carefully to debug errors in your code!
