---
title: "EXACTLY WHAT YOU MEAN"
date: 2018-01-18T21:47:16-08:00
draft: false
---

*'It's taking a long time to get out of the maze.' {{< he >}} thought 'I need to think of a better way.'*

With the code we wrote in the last step our green square jittered back and forth.  For example, if the green square was going down a corridor in the maze like this:

![Corridor](/images/corridor.png)

As it moved down the corridor, each step it would be just as likely to turn around and go back the way it came as continue on.  That's silly since we should at least explore to the end of the corridor before turning around.  It would also be just as likely to try to walk into the walls on either side as walk down the corridor.  So '*choosing a completely random square to move to next*' is not really what we mean by '*randomly walking through a maze*'.

What we really mean is more like '*Choose the path randomly, but don't turn around for no reason and don't walk into walls*'.  But we need to say it more precisely, so how about '*When choosing the next square to move to, pick one of the surrounding squares at random that is not blocked by a wall.  The chosen square should not be the way we just came from, unless we are at a dead end and its the only choice.*'.  That is probably precise enough to turn into code.

{{< notice red >}}Delete the lines of code in your <strong>GetMoves</strong> function so that we can try a different approach!{{< /notice >}}

### How Do You Eat An Elephant?

'*How do you eat an Elephant?  One bite at a time.*' so the saying goes.  In other words, we have to break big tasks down into smaller tasks.  Our big task is '*Choose one of the surrounding squares at random that is not blocked by a wall.  The chosen square should not be the way we just came from, unless we are at a dead end and its the only choice.*'.  We can write that differently as a set of steps:

1. If we are at a dead end then move back the way we came.
1. If we are not at a dead end then make a list of directions we can move from our current square (UP, DOWN, LEFT, RIGHT).
1. Only add each direction to the list if it is not blocked by a wall.
1. Only add each direction to the list if it doesn't move us back the way we came.
1. Choose one of the directions in the list at random and move in that direction.

It takes practice to be able to break things down into steps like this, but as we write more code it will become easier.

### Asking Questions

Now that we have figured out what we want to do, we need to write it down as code in our program.  But, for example, to '*choose a surrounding square that is not blocked by a wall*' we need to be able to ask questions like '*Can we move to the square to the right, or is it blocked by a wall?*'.  To help us ask those questions, the `maze` package gives us some helper functions:

{{< highlight go >}}
func Move(maze *Maze, direction Direction)
    Moves the player in the direction specified. Will not move the player if
    there is a wall blocking that direction.

func CanMove(maze *Maze, direction Direction) bool
    Returns true if the player can move in the direction specified, or false
    if there is a wall blocking that direction.

func AntiClockwise(direction Direction) Direction
    Returns the direction anti-clockwise to the one specified.

func Clockwise(direction Direction) Direction
    Returns the direction clockwise to the one specified.

func Opposite(direction Direction) Direction
    Returns the direction opposite to the one specified.

func GetLastMove(maze *Maze) Direction
    Returns the Direction that the player moved to enter this square.
    maze.Opposite of the returned direction would move the player back to
    the original square.
{{< /highlight >}}

As we write our code, we will see how to use these functions to ask questions.

### Steps

Lets write code for each of the steps in turn: 

1. If we are at a dead end then move back the way we came.
1. If we are not at a dead end then make a list of directions we can move from our current square (UP, DOWN, LEFT, RIGHT).
1. Only add each direction to the list if it is not blocked by a wall.
1. Only add each direction to the list if it doesn't move us back the way we came.
1. Choose one of the directions in the list at random and move in that direction.

#### True And False
<p style="margin-left: 4em">1. If we are at a dead end then move back the way we came.</p>

Step one is called an **if statement** and we can break it into two parts, the **if** part ('*if we are at a dead end*') and the **then** part ('*then move back the way we came*').  In Go we can write **if statements** like this:

{{< highlight go >}}
  if /* if part goes here */ {
    /* then part goes here */
  }
{{< /highlight >}}

{{< notice blue >}}Note: True/false questions in programming are called 'boolean' questions.  We can figure out the answer to boolean questions using boolean logic.{{< /notice >}}

The **if** part is always a true/false question like `*are we at a dead end or not?*'.  But we must figure out how to write that question as code.

We start by looking at the helper functions that the `maze` package gives us. Checking for a dead end would be easy if the `maze` package gave us a function like `maze.AreWeAtADeadEnd(m)`, but it doesn't. It does give us a function to see if a direction is blocked by a wall (`CanMove(m *Maze, direction Direction) bool`).

We can use the `maze.CanMove` function to work out if we are at a dead end because, in a maze, a dead end is when there is a wall in front of us, to our left and to our right.  We don't need to check behind us because we just came from that direction - there can't be a wall there!  Imagine going into this dead end from the right:

![dead end](/images/dead-end.png)

To check if there is a wall in **front**, to our **right** and to our **left**, we need to get those directions.  They depend on the direction we moved to get into our current square, so we can use the `maze.GetLastMove`, `maze.Opposite`, `maze.Clockwise` and `maze.AntiClockwise` functions.  Like this:

{{< highlight go >}}
    forward := maze.GetLastMove(m)
    back := maze.Opposite(forward)
    left := maze.AntiClockwise(forward)
    right := maze.Clockwise(forward)
{{< /highlight >}}

{{< notice red >}}Add these lines to your <strong>GetMoves</strong> function now!{{< /notice >}}

{{< notice blue >}}Note: We use <strong>maze.Clockwise</strong> to get our <strong>right</strong> direction because, if you imagine spinning yourself clockwise (looking down on yourself from above) you end up facing to your right.  The same with <strong>maze.AntiClockwise</strong> and <strong>left</strong>.{{< /notice >}}

Now we can create variables to remember if we can move forward, left and right:

{{< highlight go >}}
    canMoveForward := maze.CanMove(m, forward)
    canMoveLeft := maze.CanMove(m, left)
    canMoveRight := maze.CanMove(m, right)
{{< /highlight >}}

{{< notice red >}}Add these lines to your <strong>GetMoves</strong> function now!{{< /notice >}}

Now we can combine them into a variable to that remembers if this is a dead end.  To do that we will need **&&** and **!** which as used with true/false variables:

{{< highlight go >}}
    isDeadEnd := !canMoveForward && !canMoveLeft && !canMoveRight
{{< /highlight >}}

The **!** symbol means **NOT**, so `!canMoveForward` means the opposite of `canMoveForward` - in other words, it means **cannot move forward**.  We want to know if there is a wall in front of us, which is the opposite of whether we can move forward.  So we use **!** to give us the opposite of `canMoveForward`.

The **&&** symbol means **AND**, so `!canMoveForward && !canMoveLeft && !canMoveRight` means **NOT(canMoveForward) AND NOT(canMoveLeft) AND NOT(canMoveRight)**.  Well, if we can't move forward and we can't move left and we can't move right then we are at a dead end!

{{< notice red >}}Add the <strong>isDeadEnd</strong> line to your <strong>GetMoves</strong> function now!{{< /notice >}}

Now we ready to write the if statement for step 1 '*If we are at a dead end then move back the way we came.*':

{{< highlight go >}}
    if isDeadEnd {
        maze.Move(m, back)
    } else {
        /* if its not a dead end... */
    }
{{< /highlight >}}

{{< notice red >}}Add these lines to your <strong>GetMoves</strong> function now!{{< /notice >}}

#### Not Dead Yet

<p style="margin-left: 4em">2. If we are not at a dead end then make a list of directions we can move from our current square (UP, DOWN, LEFT, RIGHT).<br/>
    3. Only add each direction to the list if it is not blocked by a wall.<br/>
    4. Only add each direction to the list if it doesn’t move us back the way we came.<br/>
    5. Choose one of the directions in the list at random and move in that direction.</p>

We can handle the rest of the steps almost the same way as our last version - we can create a list of directions and choose one at random.  But instead of creating a list of all four directions, we will create an empty list and only add directions if they are not blocked by a wall, and they are not back the way we came.

So we start by creating an empty slice of directions (in the `else` branch of our code).  This should be familiar from the last version of our code:

{{< highlight go >}}
    directions := []maze.Direction {} 
{{< /highlight >}}

{{< notice red >}}Add these lines to your <strong>GetMoves</strong> function now!{{< /notice >}}

Then we add each direction to the slice using the [append function](https://tour.golang.org/moretypes/15) if we can move in that direction:

{{< highlight go >}}
    if maze.CanMove(m, forward) {
        directions = append(directions, forward)
    }

    if maze.CanMove(m, left) {
        directions = append(directions, left)
    }

    if maze.CanMove(m, right) {
        directions = append(directions, right)
    }
{{< /highlight >}}

{{< notice red >}}Add these lines to your <strong>GetMoves</strong> function now!{{< /notice >}}

Finally we choose a random number between 0 and the length of the list and the move in the chosen direction:

{{< highlight go >}}
    random := rand.Intn(len(directions))
    maze.Move(m, directions[random])
{{< /highlight >}}

{{< notice red >}}Add these lines to your <strong>GetMoves</strong> function now!{{< /notice >}}

We use `len(directions)` to get the length of the slice because we don't know how many directions will be in it when we are writing the code.

### Build and Run

Once we're done, our `GetMoves` function should look like this:

{{< highlight go >}}
func GetMoves(m *maze.Maze) {
    forward := maze.GetLastMove(m)
    back := maze.Opposite(forward)
    left := maze.AntiClockwise(forward)
    right := maze.Clockwise(forward)

    canMoveForward := maze.CanMove(m, forward)
    canMoveLeft := maze.CanMove(m, left)
    canMoveRight := maze.CanMove(m, right)
    isDeadEnd := !canMoveForward && !canMoveLeft && !canMoveRight

    if isDeadEnd {
        maze.Move(m, back)
    } else {
        directions := []maze.Direction {}

        if maze.CanMove(m, forward) {
            directions = append(directions, forward)
        }

        if maze.CanMove(m, left) {
            directions = append(directions, left)
        }

        if maze.CanMove(m, right) {
            directions = append(directions, right)
        }

        random := rand.Intn(len(directions))
        maze.Move(m, directions[random])
    }
}
{{< /highlight >}}

Lets **build** and **run** our code!

{{< notice green >}}
DON'T PANIC!
If you get an error when you run <strong>go build</strong>, take a look at the <a href="/troubleshooting.html">troubleshooting guide</a> to work out what to do.
{{< /notice >}}

We should see our green square is still walking around randomly, but at least now its not turning around for no reason in the middle of corridors and it spends less time bumping into walls!
