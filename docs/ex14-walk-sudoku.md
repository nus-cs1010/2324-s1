# Exercise 14: Walk, Sudoku

### Deadline

This is an ungraded exercise.

### Learning Objectives

- Learn to use non-linear recursion

### Grading

This is an ungraded exercise.

## Question 1: Walk

any cities in the world, such as New York City, are laid
out as a grid, with streets running in the north-south
directions and east-west directions.  Buildings between two
streets are called _blocks_.

Suppose we start at a junction of two streets, and we wish
to walk to another junction that is $y$ blocks to the north
and $x$ blocks to the east, how many possible paths are
there to walk to the destination?

For example, suppose we want to walk to a junction that is
one block to the north and one block to the east, and we can
only walk eastward and northward, there are two possible
paths.  We can walk east for one block, then walk north for
another block.  Or, we can walk north first for one block,
then walk east for another block.

The figure below illustrates the scenario, where we are
positioned at source D and we wish to go to destination B.
The two possible paths are DAB and DEB.

```
A---B---C
|   |   |
D---E---F
```

Suppose now we want to walk to C, a junction that is two
blocks to the east, and one block to the north.  There are
three different possible paths.  The three paths are DABC,
DEBC, DEFC.

Write a program `walk` that reads in two integers $x$ and $y$
$(x \ge 0, y >= 0)$, and prints the number of possible paths we
can walk to the destination that is $x$ block to the east and
$y$ block to the north.

Your solution must take no more than $O(xy)$ time.

Hint: Think recursively, but solve iteratively.

## Sample Run
```
ooiwt@pe111:~/as08-ooiwt$ walk
10 0
1
ooiwt@pe111:~/as08-ooiwt$ walk
1 1
2
ooiwt@pe111:~/as08-ooiwt$ walk
2 2
6
ooiwt@pe111:~/as08-ooiwt$ walk
20 20
137846528820
```

## Question 2: Sudoku

This question is inspired by The Prime Minister of Singapore,
Lee Hsien Loong, who [once wrote a sudoku solver in C++](https://www.facebook.com/leehsienloong/photos/a.344710778924968.83425.125845680811480/905828379479869/?type=1&theater):

In the game of sudoku, we are given a $9 \times 9$ grid, with some
of the cells filled with numbers 1 to 9.  We must fill up
the rest of the cells with 1 to 9, such that no number
repeats itself in each row, in each column, and in each
of the nine \$3 \times 3$ subgrids.

Write a program, `sudoku.c`, that reads in a sudoku
puzzle, and search for a solution to the puzzle, using
the following algorithm:

- Start from the top left, and scan left to right, top
  to bottom.

- If an empty cell is encountered, try to fill it up with
  a digit, starting with 1, 2, .. until 9.  For each digit
  we try, check if it is possible to find a solution, and
  if so, solve the updated puzzle recursively.

- If a solution is found, print the solution and stop
  searching.  Otherwise, (backtrack and) try a different
  digit, until either we have tried all digits or a solution
  is found.

The input to the program consists of 9 lines, 9 characters
in each line, representing the puzzle.  The input
contains only the digits 1 to 9 and the `.` character,
which represents the empty cell.

The output contains the steps of the search, printed with
`print_sudoku` method provided.  At the end of the program,
either the solution is printed or the string "no solution"
if the sudoku cannot be solved.

You can try to run the program `~cs1010/ex14/sudoku` to see the
animation of how the solution is expected to work.
