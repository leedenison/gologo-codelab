---
title: "UNDERSTANDING THE CODE"
date: 2018-01-18T13:47:13-08:00
draft: false
---

*{{< capHe >}} wondered how the maze got here.  It seems solid enough.  Its stone surface covered in strange symbols.*

Before we try to find our way out of the maze, lets make sure we understand the code we have so far.

### package

The first line of the code is always the package declaration:

{{< highlight go >}}
package main
{{< /highlight >}}

Our package can be called anything we want, but we have to call it something.  Packages are mostly useful when we have big projects with lots of code, but we still have to have a package name even when our project is small.

### import

The import lines let us use packages that other people have written in our code:

{{< highlight go >}}
import (
    "github.com/leedenison/gologo/maze"
)
{{< /highlight >}}

In `maze.go` we only import one package.  The last part of the package name (eg. `maze`) is the important part and tells us what the package is called.  It is the name we will use when we want to use the package.

The first part of the package name (eg. `github.com/leedenison/gologo/`) is less important.  It just tells Go where it should get the package from.

### func GetMoves

Next we define a function called `GetMoves`:

{{< highlight go >}}
func GetMoves(m *maze.Maze) {
}
{{< /highlight >}}

You might remember functions from [a tour of Go](https://tour.golang.org/basics/4).  Functions are like mini programs that we can run whenever we want.  So they are useful if we want to do the same thing over and over again - it saves us writing the same code many times.  This `GetMoves` function doesn't do anything yet (it doesn't have any code between the curly brackets `{}`).  But later on we are going to use it to decide what moves we want to make in the maze.

{{< notice blue >}}Note: When you run a function we say that you <strong>call</strong> it.  For example we might say "The first thing our program does is call the main function."{{< /notice >}}

### Variables

In [a tour of Go](https://tour.golang.org/basics/8), we saw that we use variables to __control__ things and to __remember__ things in our program.  For example, we could write some code for our `GetMoves` function like this:

{{< highlight go >}}
func GetMoves(m *maze.Maze) {
  foo := 0
  bar := ""

  if (foo > 3) {
    bar = "if branch"
  } else {
    bar = "else branch"
  }
}
{{< /highlight >}}

We have created two variables `foo` and `bar` (in Go we use `:=` to create a variable for the first time and set its value, after that we just use `=` to set its value to something else).   We can __control__ whether our program goes through the `if` branch or the `else` branch of our function by setting the variable `foo` to a number bigger than 3 or smaller than 3.  We can __remember__ which branch we went into using the `bar` variable.  Later in the program we would be able to check the value of bar to see what we did. 

Still looking at the `GetMoves` function, notice the part that comes after the function name that says `(m *maze.Maze)`.  `m` is a variable that controls which maze are going to move inside of.  `m` is the name of the variable and `*maze.Maze` tells us what type of variable `m` is.  Variables can be all different types: numbers, words, true/false and, in this case, an entire maze!  We will find out how to use `m` later to make the moves we want to make.

{{< notice blue >}}Note: The <strong>maze.Maze</strong> variable type has a <strong>*</strong> in front of it.  This means it is a pointer.  For now we won't worry much about what a pointer is. Pointers don't change how we use the variable very much, so we treat <strong>m</strong> the same whether it is a pointer or not.{{< /notice >}}

### func main

Lastly we have our main function:

{{< highlight go >}}
func main() {
    maze.Run(10, 10, GetMoves)
}
{{< /highlight >}}

All Go programs have a main function which is the first function to get run.  In this case our main function has one line that calls the `maze.Run` function.  Really its just the `Run` function but we have to put `maze.` in front to tell Go to look for it in the `maze` package.

We are passing three variables to the `maze.Run` function:

- The first is a number (10) saying how many squares wide our maze should be.
- The second is also a number (also 10) saying how many squares tall our maze should be.
- The third is the name of a function (`GetMoves`).  This is the function we want Go to call to find out what moves we should make to find our way out of the maze.

*That's enough thinking time {{< he >}} thought to {{< himself >}}.  Time to do something...*
