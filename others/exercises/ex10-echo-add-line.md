# Exercise 10: Echo, Add, Line

### Deadline

This is an ungraded exercise.

### Prerequisite

- Completed [Exercise 9](ex09-len-cat-find.md)

### Learning Objectives

- Learn how to use 2D arrays in C

### Grading

This is an ungraded exercise.

## Question 1: Echo

Write a program `echo` that reads a 2D matrix of integers and prints the matrix back to the standard output.

The input consists of the following:
- the first two numbers correspond to $R$, the number of rows, and $C$, the number of columns of the matrix respectively
- the next line contains $R$ x $C$ integers.

Print to the standard output:
- $R$ rows of numbers, each row contains $C$ integers,
  corresponding to the rows of the matrix.

Your code should contain two functions:

```C
long **read_matrix(size_t nrows, size_t ncols) {
  :
}
```
to allocate, read, and return the matrix (return NULL if allocation
fails).

and

```C
void print_matrix(size_t nrows, size_t ncols, long **matrix) {
  :
}
```
to print the matrix.

### Sample Run

```
ooiwt@pe101:~$ ./echo
3 4
1 2 3 4 5 6 7 8 9 10 11 12
1 2 3 4
5 6 7 8
9 10 11 12
```

## Question 2: Add

Write a program `add` that reads two 3x3 matrices $M$ and $N$
from the standard inputs, and prints the resulting matrix
$M + N$ to the standard output.

For example,

$\begin{pmatrix}
3 & 1 & 2\\
0 & 4 & 1\\
3 & 5 & 0 \\
\end{pmatrix}$ + 
$\begin{pmatrix}
1 & -1 & 3\\
-1 & 2 & 0\\
0 & 1 & 3 \\
\end{pmatrix}$
=
$\begin{pmatrix}
4 & 0 & 5\\
-1 & 6 & 1\\
3 & 6 & 3 \\
\end{pmatrix}$


If the two input matrices are
```
3 1 2
0 4 1
3 5 0
```
and
```
1 -1 3
-1 2 0
0 1 3
```
then print to the standard output
```
4 0 5
-1 6 1
3 6 3
```

### Sample Run

```
ooiwt@pe101:~$ ./add
3 1 2
0 4 1
3 5 0
1 -1 3
-1 2 0
0 1 3
4 0 5
-1 6 1
3 6 3
```

## Question 3: Line

Write a program that draws a white line on an image with a black background of sizes $W$ and $H$.

The line is specified as two coordinates ($x_1$, $y_1$) and
($x_2$, $y_2$).  The top left corner has a coordinate (0,0).
The x coordinates increase to the right; the y coordinates 
increase to the bottom.

Let ($x$, $y$) be any point that lies on this line, then
we have:

$$ \frac{y-y_1}{y_2 - y1} = \frac{x-x_1}{x_2-x_1}$$

We can rearrange and get:

$$y = \frac{y_2-y_1}{x_2 - x1}(x-x_1) + y_1$$

Let

$$m = \frac{y_2 - y_1}{x_2 - x_1}$$

be the slope of the line.

If the line passes through ($x$, $y$), then it passes through
($x + 1$, $y + m$).

Mathematically, x and y are floating-point numbers.  But,
when we draw the line in an image, we have to round them
into integers.  This process is called _rasterization_ in
computer graphics.

To draw a rasterized straight line, we can then loop
through different values of $x$, from $x_1$ to $x_2$, and
update the corresponding value of $y$ (by repeatedly adding
$m$ every time $x$ increments by 1).

Since $y$ is a floating-point number, we round $y$ to the
nearest integer and draw the line at pixel ($x, y$).  You may
find the function `round` from `math.h` useful.

We will draw the output in an image format called PGM.
Each pixel of the image has a value ranging between 0 and
255.  For this question, we draw images with two colors:
0 for the black background and 255 for the white line.

Your program should read from the standard input:

- Two positive integers, $W$ and $H$ ($W$ > 1, $H$ > 1)
- Four non-negative integers, $x_1$, $y_1$, $x_2$, $y_2$, that
  corresponds to the two endpoints ($x_1$, $y_1$) and
  ($x_2$, $y_2$) of the line.  The slope of the line is
  guaranteed to be between -1 and 1.  Furthermore,
  $x_2$ > $x_1$.

Your program should print to the standard output,

- The first line contains the string "P2"

- The second line contains two integers, the width $W$ and
  height $H$ of the image

- The third line contains the number 255

- The rest of the output contains $W\times H$ integers, each 
  having a value of either 0 or 255.  Each value corresponds
  to a pixel in the output image.  The order of the pixels is 
  from left to right, top to bottom.  Each row of the
  image must be printed on a separate line.

If you pipe your output into ~cs1010/bin/viu, you can
visualize your output as an image.

```
ooiwt@pe101:~$ ./line < inputs/line.3.in | ~cs1010/bin/viu -
```

Note that if your image to too big to fit your terminal, `viu`
will resize your image.

### Sample Runs

```
ooiwt@pe101:~$ ./line
4 4
0 0 3 3
P2
4 4
255
255 0 0 0
0 255 0 0
0 0 255 0
0 0 0 255
ooiwt@pe101:~$ ./line
4 4
0 2 3 2
P2
4 4
255
0 0 0 0
0 0 0 0
255 255 255 255
0 0 0 0
```
