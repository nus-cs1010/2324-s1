# Assignment 7: Fill, Maze

### Deadline

1 November 2022 (Tuesday), 23:59 PM

### Prerequisite

- Understand how to search (branch-and-bound, backtracking) recursively
- Able to analyze the running time of an algorithm

### Learning Objectives

- Able to model a problem as a search problem and solve it recursively 

### Grading

This assignment contributes to **4.0%** of your final grade.  The total marks for this assignment are 40 marks.

We may deduct up to 1 mark _for each question_ for style violation.  Ensure that your code is neat and readable, adhering to the CS1010 coding convention.

Mark are no longer allocated to documentation. We may deduct up to 2 marks _for each question_ for missing documnetation.  Your code should follow the good practices of breaking down your solution into small functions. Document each function (except `main`) following the Doxygen documentation format, as outlined [here](../guides/documentation.md).

Each question has an efficiency requirement.  You need to solve the problem within the given running time to get the efficiency marks.  _We may deduct marks if your code is inefficient (e.g., perform redundant or duplicate work), even if its running time is within the given bound_.  Note that if your approach to solve the problem is incorrect, no efficiency marks will be given.

The rest of the marks are allocated to correctness -- this includes the correctness of syntax, practices, approach, and logic, and whether you correctly follow our instructions.  Note that _even if your solution produces the correct output every time, it may not get full marks if the approach is wrong._

## Question 1: Fill (20 marks)

One of the basic operations in drawing software is the
fill operation (aka bucket fill).  You will implement this
operation in this assignment.

An image can be considered a 2D array of pixels.
Each pixel has a color, represented by three integers,
corresponding to the red, green, and blue components of the color.   We say that two pixels are adjacent to each
other if they share a common edge.  So a pixel can have
at most four adjacent pixels.

We denote the top left pixel to be (0,0).  The indices
increase towards the right and down.  For instance, if
we have an image of width 20 by 30, then the top-right
pixel has the coordinate (0, 19); the bottom-left pixel
has the coordinate (29, 0); and the bottom-right has the coordinate {--(19, 29)--} {++(29, 19)++}.

An image can be divided into one or more segments.
A segment is a set of pixels (i) with the same color, and
(ii) given any two pixels $p$ and $q$ in a segment, we can
find a series of adjacent pixels in the segment going from
$p$ to $q$.

The fill operation takes in as an input the ($x$,$y$)
coordinate of a pixel $p$, and a color $c$ to fill.  The
operation then replaces the color of all pixels belonging 
to the same segment as $p$ with the given color $c$.

Write a program, `fill.c`, that reads from standard input
two positive integers $m$ and $n$ in the first line.  $m$ refers
to the number of rows, and $n$ is the number of columns.

Then read $3mn$ integers representing an $m$ by $n$ image
from the standard inputs.
Each tuple of 3 integers refers to the color of a pixel.
The pixels are read from left to right, top to bottom.
Thus, the first three integers refer to the color of the pixel 
at (0, 0); the next three integers refer to the color of
the pixel at (0, 1), etc.

The next input is a positive integer $q$, which is the number
of fill operations.  Following this, there are $q$ lines.
Each line has two integers $x$, $y$, and three integers, $r$,
$g$, and $b$, corresponding to a color.  Each line specifies
a fill operation, to fill the segmented pixel $(x, y)$ is in
with the color $(r, g, b)$.

The program shall print to standard output, the image
after executing all the given fill operations.  The image
should be printed in the following format:

- The first line starts with a string "P3", followed by a
  space, followed by two integers, $n$, and $m$, that correspond to the width and height of the image.  This is followed
  by a space, and then number 255.
- The remaining lines contain the color of each pixel,
  one color (i.e., three integers) per line.  The pixels
  should be printed from left to right, top to bottom.

An example output:
```
P3 2 2 255
0 0 0
0 10 79
140 120 30
50 30 255
```

You should use recursion to implement the fill operation.

Your solution must take no more than $O(mnq)$ time.

You can visualize the resulting image using the `viu` command:

```
./fill < inputs/fill.3.in | ~cs1010/bin/viu -``
```

Note that `viu` might rescale your image if your terminal
is too small.

### Tips

Similar to `social`, your code would be simpler if you
separate the representation of the image from the
underlying 2D array.  Writing functions to retrieve,
update, and compare the color of a pixel of an image
would make your code much simpler and easier to read,
write, and debug.

### Grading Criteria

The grading criteria for this question is:

|              | Marks |
|--------------|-------|
| Correctness  | 12    |
| Efficiency   | 8     |

Your solution must take no more than $O(mnq)$ time to obtain
the full efficiency marks.

## Question 2: Maze (20 marks)

Agent Scully woke up and found herself in the dark.  She
figured out that she is in a maze.  She has to find her way
out if there is one!

The maze can be simplified as a grid of ($m \times n$) cells. Each
cell can be of the following:

1. '#' denotes a segment of a wall.  No one can pass through
   the walls.

2. '.' denotes an empty space

3. '@' denotes where Scully is currently. It is also an
   empty space

Anytime Scully reaches a border cell (a cell in either the
top-most or bottom-most row or left-most or right-most
column), she escapes the maze and can go save her partner
Agent Mulder.  She can only move from one empty space to
another adjacent cell in one step.  Two cells are adjacent
if they share a common edge.

Scully took CS1010, and she got a concrete plan to seek a
way out by trial and error.  She follows **strictly** the
following strategy to find a way through the maze starting
from her initial position.  At each time step,

1. She looks for an empty adjacent cell that has never been
visited yet, in the sequence of up/right/down/left to the
current cell she is at.  If there is an empty adjacent cell,
she moves to that cell.  The cell she moves to is now
visited.

2. If no empty, unvisited, adjacent cell exists, she
backtracks on the path that she comes from, moving one step
back, and repeats 1 again.

In this way, Scully systematically explores the maze,
with no repetitive visit of any cell more than once except
when she backtracks.  She will stop when successfully
escaped the maze, or finds that there is no way out after
backtracking all the way back to the original position.  She
is completely trapped within the maze and now must wait for
her partner Agent Mulder to come and free her.

Write a program `maze.c`, that reads from standard input.
First, read two positive integers $m$ and $n$, followed by $m$
lines of $n$ characters in each line that represents the maze
setup.  One and only one `@` will be present in the maze
setup.

The program then prints, to standard output, an animation of
$k$ iterations. The output should only contain $m$ rows with
$n$ characters in each row, with an additional row at last.
Similarly, you must use `#` to represent a wall, a `.` to represent empty space, and `@` to represent where Scully is
at.  After printing the maze, your program prints the number
of steps that Scully has made.

You should use recursion to explore the maze and look for a
way out.

Here is an example.  The following is the starting position
of Scully and the maze.

```
###########
#.#.....#.#
#.#####.#.#
#.#...#.#.#
#.#.#.....#
#.#.##.#.##
#@#.#..#..#
#...#.#.#..
###########
0
```

Scully firstly moves five steps up:
```
###########
#@#.....#.#
#.#####.#.#
#.#...#.#.#
#.#.#.....#
#.#.##.#.##
#.#.#..#..#
#...#.#.#..
###########
5
```

At this point, Scully is stuck since there is no more
adjacent empty cell that has not been visited.  Scully then
backtracks:
```
###########
#.#.....#.#
#.#####.#.#
#.#...#.#.#
#.#.#.....#
#.#.##.#.##
#@#.#..#..#
#...#.#.#..
###########
10
```

Scully then moves down:
```
###########
#.#.....#.#
#.#####.#.#
#.#...#.#.#
#.#.#.....#
#.#.##.#.##
#.#.#..#..#
#@..#.#.#..
###########
11
```

Then right:
```
###########
#.#.....#.#
#.#####.#.#
#.#...#.#.#
#.#.#.....#
#.#.##.#.##
#.#.#..#..#
#..@#.#.#..
###########
13
```

Then up:
```
###########
#.#.....#.#
#.#####.#.#
#.#@..#.#.#
#.#.#.....#
#.#.##.#.##
#.#.#..#..#
#...#.#.#..
###########
17
```

Then right (two steps) and then down (two steps) and then
right (two steps):

```
###########
#.#.....#.#
#.#####.#.#
#.#...#.#.#
#.#.#..@..#
#.#.##.#.##
#.#.#..#..#
#...#.#.#..
###########
22
```

Then Scully moves up and left, and she is stuck again.

```
###########
#.#@....#.#
#.#####.#.#
#.#...#.#.#
#.#.#.....#
#.#.##.#.##
#.#.#..#..#
#...#.#.#..
###########
29
```

At this point she backtracks:

```
###########
#.#.....#.#
#.#####.#.#
#.#...#.#.#
#.#.#..@..#
#.#.##.#.##
#.#.#..#..#
#...#.#.#..
###########
36
```

Moves right, and up, and stuck again!

```
###########
#.#.....#@#
#.#####.#.#
#.#...#.#.#
#.#.#.....#
#.#.##.#.##
#.#.#..#..#
#...#.#.#..
###########
41
```
She backtracks again,

```
###########
#.#.....#.#
#.#####.#.#
#.#...#.#.#
#.#.#...@.#
#.#.##.#.##
#.#.#..#..#
#...#.#.#..
###########
45
```

This time she found her way out!

```
###########
#.#.....#.#
#.#####.#.#
#.#...#.#.#
#.#.#.....#
#.#.##.#.##
#.#.#..#..#
#...#.#.#.@
###########
50
```

It took her a total of 50 steps to exit the maze.

### Grading Criteria

The grading criteria for this question are:

|              | Marks |
|--------------|-------|
| Correctness  | 12    |
| Efficiency   |  8    |

Your solution must take no more than $O(mn)$ time to obtain
the full efficiency marks.

**Hint:** You need to strictly follow the described strategy and
sequence of exploration. Do not forget to print the initial
maze and the final maze.

### Animation on Screen

We have provided a function `print_maze` to draw the maze in the skeleton
file.

## Sample Run

You can use the sample program `~cs1010/as07/maze` on the given
inputs to view the animation.

Note that how the maze is printed on the standard output is
different from when it is written to a file.
