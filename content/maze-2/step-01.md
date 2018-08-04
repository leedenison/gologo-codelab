---
title: "FEET ON THE GROUND"
date: 2018-01-27T13:07:48-08:00
draft: false
---

*"Things would be so much simpler if I could fly", {{< he >}} thought to {{< himself >}} "I could look down and see the whole maze.  Finding the way out would be easy!"  But as things were {{< he >}} would have to find {{< his >}} way out on the ground.*

### So Random

Imagine that you can't see the whole maze.  In that case we would have to write the `GetMoves` function so that it could solve _any_ maze, not just the one we have.  That's actually a good thing because a function that can solve any maze is more useful than a function that solves only one maze.  It does also mean that our `GetMoves` function must become more complex.

We can tell Go to generate a different, random maze every time we run the program.  First we add an `init` function which gets run at the beginning of the program and chooses a random seed based on the time that the program was run:

{{< highlight go >}}
func init() {
    rand.Seed(time.Now().UTC().UnixNano())
}
{{< /highlight >}}

{{< notice red >}}Update your code now!{{< /notice >}}

And since we are now using the `rand` packge and the `time` package, we need to add those to our imports:

{{< highlight go >}}
import (
    "math/rand"
    "time"
    "github.com/leedenison/gologo/maze"
)
{{< /highlight >}}

{{< notice red >}}Update your code now!{{< /notice >}}

### Build and Run

Now we don't know what the maze will be until we run the program.  So it's like we cannot fly above the maze and simply see the way out.  We have to write a `GetMoves` function that can find the way out of any maze.

Lets __build__ and __run__ the program a few times.  We should see a different maze each time and the instructions we added to solve the first maze will almost never work on these random mazes.

{{< notice green >}}
DON'T PANIC!
If you get an error when you run <strong>go build</strong>, take a look at the <a href="/troubleshooting.html">troubleshooting guide</a> to work out what to do.
{{< /notice >}}

Lets remove the instructions from the `GetMoves` function since they don't work any more.  Our `GetMoves` function should look like this when were done:

{{< highlight go >}}
func GetMoves(m *maze.Maze) {
}
{{< /highlight >}}
