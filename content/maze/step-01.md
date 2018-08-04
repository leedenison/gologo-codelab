---
title: "BUILDING THE CODE"
date: 2018-01-18T21:27:53-08:00
draft: false
---

*The kid found {{< himself >}} in a maze.  Why?  That was a good question, but first {{< he >}} needed to get out.*

To see the maze we first need to build and run the code.  Open the `maze.go` file and you should see the code to create a maze ten squares wide and ten tall:

{{< highlight go >}}
package main

import (
    "github.com/leedenison/gologo/maze"
)

func GetMoves(m *maze.Maze) {
}

func main() {
    maze.Run(10, 10, GetMoves)
}
{{< /highlight >}}

## GO BUILD

To build the code we need to run the build command:

{{< highlight shell >}}
$ go build maze.go
{{< /highlight >}}

If there are no mistakes in the code, this will create a runnable file called `maze.exe`.  Sometimes we will make mistakes.  When that happens `go build` will show us an error message which tells us what we did wrong.  We will see that later, but for now lets run `maze.exe`.

{{< highlight shell >}}
$ ./maze.exe 
{{< /highlight >}}

You should see a maze that looks like this:

![First Maze](/images/first-maze.png)

The green square shows us where we start in the maze.  The red square shows us where we need to get to.

{{< notice yellow >}}
Note: From now on when we need to build and run the code we will follow the steps above.
{{< /notice >}}

