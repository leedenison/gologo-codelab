---
title: "START SIMPLE"
date: 2018-01-17T08:31:38-08:00
draft: false
---

*The kid could see how to get out.  Should be simple, right?*

### Move! Move! Move!

To get out of the maze we need to add code to the `GetMoves` function which tells the program which way to move the green square.  There are four functions we can use to move in the maze:

{{< highlight go >}}
maze.MoveDown(m *maze.Maze)
maze.MoveUp(m *maze.Maze)
maze.MoveLeft(m *maze.Maze)
maze.MoveRight(m *maze.Maze)
{{< /highlight >}}

For example, we can move to the left and then move to the right by adding these lines to our `GetMoves` function:

{{< highlight go >}}
func GetMoves(m *maze.Maze) {
    maze.MoveLeft(m)
    maze.MoveRight(m)
}
{{< /highlight >}}

{{< notice red >}}Update your code now!{{< /notice >}}

Notice that we have to pass our maze `m` to the functions to tell Go which maze are moving around in.

Now we can __build__ and __run__ our code and you should see the green square moving left and right.  It keeps going because after it moves left and then right, it still hasn't reached the end of the maze.  So it calls `GetMoves` again which tells it to move left and right again.  It repeats this forever until we close the program.

{{< notice green >}}
DON'T PANIC!
If you get an error when you run <strong>go build</strong>, take a look at the <a href="/troubleshooting.html">troubleshooting guide</a> to work out what to do.
{{< /notice >}}

### Time To Go

Now all you need to do is put the correct moves into the `GetMoves` function and you can solve the maze.

Change the code inside the `GetMoves` function to be the correct list of moves to get from the green square to the red square in the maze.  Then __build__ and __run__ the program to see if it works.
