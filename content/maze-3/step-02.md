---
title: "BREAK IT DOWN"
date: 2018-01-28T16:49:58-08:00
draft: false
---

Hopefully you came up with some steps for the `GetMoves` function which are a bit like this:

1. If we can move left (anti-clockwise from our last move), we move left.
1. If we cannot move left, but we can move forward (same direction as our last move), we move forward.
1. If we cannot move left or forward, but we can move right (clockwise from our last move), we move right.
1. If we cannot move left, forward or right, we move back the way we came.

{{< notice red >}}Try to write the code for these steps in the <strong>GetMoves</strong> function now!{{< /notice >}}

You should:

- start with getting each of the directions as in the last version of `GetMoves` that we wrote (you can go back and look at the last page to remind yourself how to do it).
- write some if statements which use `maze.CanMove` to carry out the steps above.
- make sure you only add one move each time `GetMoves` is called (its easy to accidentally add several moves each time!).

{{< notice yellow >}}Note: We shouldn't need to create a list of directions this time!{{< /notice >}}

### Build and Run

When you have written the code, **build** and **run** it to see if it works!  Don't worry if it doesn't work at first!  Watch what the green square does and see if you can figure out what you need to change in your code!

{{< notice green >}}DON'T PANIC! If you get an error when you run <strong>go build</strong>, take a look at the <a href="/troubleshooting.html">troubleshooting guide</a> to work out what to do.{{< /notice >}}


*{{< capHe >}} was so happy to see the exit of the maze, and really impressed that {{< he >}} figured out how to find the way out.  'I will have to write down how to do it and send it to my uncle!' {{< he >}} thought.  As {{< he >}} got close to the way out of the maze, {{< he >}} started to hear a strange noise of stone grinding on stone.  When {{< he >}} reached the corner he saw a strange old man stood in front of three stone pillars.  {{< capHe >}} thought about jumping back to hide behind the wall, but the old man had already looked up and was staring straight at {{< him >}} with sharp eyes.*

*To be continued...*
