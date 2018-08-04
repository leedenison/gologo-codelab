---
title: "WONDERING ABOUT WANDERING"
date: 2018-01-22T08:12:56-08:00
draft: false
---

*The kid thought about the best way to get out of a maze.  {{< capHe >}} remembered something about leaving a trail of breadcrumbs, but {{< he >}} didn't have any bread.  So that idea was a waste of time anyway.  {{< capHe >}} decided {{< he >}} might as well walk while {{< he >}} thought about it, so {{< he >}} set off in a direction that seemed as good as any.*

A simple way to find our way out of the maze is to walk around randomly until you find the exit.  That's easy to say, but to make a program that '*walks around randomly*', we have to think carefully about exactly what our program needs to do when it decides what move to take next.

{{< notice blue >}}Note: The secret of writing any computer program is figuring out how to say precisely what you want the program to do.  Then figuring out how to break that down in to very small steps.{{< /notice >}}

### Walking Around Exactly At Random

What exactly do we mean when we say '*walk around at random*'?  One answer would be something like '*when choosing the next square to move to, pick one of the surrounding squares at random*'.  We can write that differently as a set of steps:

1. Make a list of directions we can move from our current square (up, down, left, right).
1. Choose one of the directions in the list at random and move in that direction.

But to make a list of directions, we need to represents the up, down, left and right directions somehow.  The `maze` package gives us four constants we can use, one for each direction:

{{< highlight go >}}
  maze.UP
  maze.DOWN
  maze.LEFT
  maze.RIGHT
{{< /highlight >}}

{{< notice blue >}}Note: Constants are just like variables but they can't change the their value.  We write their names using capital letters to make it easy to see that they are constants.{{< /notice >}}

### Enlisting Lists

The easiest way to make a list of directions (or anything) in our code is to use a [Go slice](https://tour.golang.org/moretypes/7).  A slice in Go is used hold a group of things (numbers, letters, words or anything at all).  As our Go program runs, we can add more things to our slice, take things out and at any time we can check whats in the slice.  So its a great way to remember lists of stuff.  You can even take everything out leaving it empty (which can be useful sometimes!).

We can create a slice of directions by adding code to our `GetMoves` function like this:

{{< highlight go >}}
  directions := []maze.Direction {
    maze.UP,
    maze.LEFT,
    maze.RIGHT,
    maze.DOWN,
  }
{{< /highlight >}}

{{< notice red >}}Update your code now!{{< /notice >}}

Lets make sure we understand the parts of this code:

- `directions` - This is the variable we are using to remember our slice.  We'll use it again later when we want to get our chosen direction out of the slice.
- `:=` - We use `:=` instead of just `=` to set the value of `directions` because this is the first time we are using `directions`.  The extra `:` means we are creating a new variable.
- `[]maze.Direction { }` - This is how we create a new slice of directions to remember with our `directions` variable.  The `[]` part tells Go that we are creating a slice of something.  The `maze.Direction` part tells Go that we are putting `Directions` in our slice and nothing else (for example `[]int {}` would be a slice of numbers instead of a slice of directions).  If we try to put anything that is not a `Direction` in our slice Go will complain at us when we run `go build`.
- `{ maze.UP, maze.LEFT, maze.RIGHT, maze.DOWN }` - The bit inside the `{ }` tells Go what we want to put in our slice to start with.  We can change it by adding or removing stuff later in our program, but this is what we start with.  It is allowed to be empty, in which case we don't put anything inside the `{ }`, but we still need the curly brackets.

Things that we put in a slice are always numbered and we get them out by their number.  We get things out of a slice by adding square brackets with a number inside (eg. `directions[3]`).  Beware though, the numbers always start from 0 not from 1!  So, for example, if we want to get the first direction in our slice we would write:

{{< highlight go >}}
  directions[0] // We use 0 for the first direction
{{< /highlight >}}

For the second direction in our slice we would write:

{{< highlight go >}}
  directions[1]
{{< /highlight >}}

And so on.

{{< notice blue >}}Note: Never forget that the numbers for things in slices always start from 0!  It's easy to forget and make mistakes!{{< /notice >}}

### Pick A Card, Any Card

We want a random direction from our list of directions.  So we need to pick a random number between 0 and 3 (our first direction is numbered 0 and the last one is numbered 3 remember!).  To do that we use the `rand.Intn` function which picks a number **less than** the number we give it:

{{< highlight go >}}
  random := rand.Intn(4) // 4 because we want a number between 0 and 3
{{< /highlight >}}

{{< notice red >}}Update your code now!{{< /notice >}}

### Finally We Move

Lastly we can move in the randomly chosen direction using the `maze.Move` function:

{{< highlight go >}}
  maze.Move(m, directions[random])
{{< /highlight >}}

{{< notice red >}}Update your code now!{{< /notice >}}

Notice that we use the variable `random` inside the square brackets for `directions`, instead of an actual number.  This is fine because we know that `random` is a number between 0 and 3.

### Build And Run

When we're done, our code for `GetMoves` should look like this:

{{< highlight go >}}
  func Move(m *maze.Maze) {
    directions := []maze.Direction {
      maze.UP,
      maze.LEFT,
      maze.RIGHT,
      maze.DOWN,
    }

    random := rand.Intn(4)
    maze.Move(m, directions[random])
  }
{{< /highlight >}}

**Build** and **run** the code and see what happens!

{{< notice green >}}
DON'T PANIC!
If you get an error when you run <strong>go build</strong>, take a look at the <a href="/troubleshooting.html">troubleshooting guide</a> to work out what to do.
{{< /notice >}}

Notice that our green square seems a bit confused!  Don't worry, that's expected!
