# Unit 26: N Queens

## Learning Objectives

After taking this unit, students should

- be familiar with using recursion to search and prune the solution space to a problem
- be familiar with the class N queens problem and its solution

## Searching and Bracktacking

We now look at how recursion can help with solving problems that require _searching and backtracking_.  A classical example for this is the $n$-queens problem, which can be stated as given a $n \times n$ chessboard, find a possible placement of $n$ queens on the chessboard, such that the queens do not threaten each other.    In other words, there is exactly one queen in each row, in each column, and each diagonal.  The 8-queens puzzle was first published by Max Bezzel in 1848, with the first solution published Franz Nauck in 1850.
The generalized $n$-queens problems were introduced later.  It is known that there is no solution for $n = 2$ and $n = 3$, but a solution exists for $n > 3$.

If we visualize the chessboard as a 2D array, with `#` as the position of a queen, and `.` as an empty position on the board, then a solution to the 4-queens problem looks like this:

```
.#..
...#
#...
..#.
```

## Recursive Formulation

Let's see how we can formulate the problem recursively.  The first step is to simplify the problem to the most trivial case where we can solve it.  It is tempting to say that a simpler version of the problem is an $n-1$-queens problem, and so the most trivial case is 1-queen.  While the solution to 1-queen is trivial, there is no solution to both 2-queen and 3-queen problems.  Further, if we have found a solution to the $n-1$-queens problem, extending it to a solution of the $n$-queens problem is not trivial.

### Generate All Permutations

As a start, let's borrow the idea from [Unit 25](25-permutation.md) and generate all permutations of the queens' positions.  Let's label the columns `a`, `b`, `c`, .. etc.  Since we know that there must be exactly one queen in each row, and one queen in each column, the positions of the queens can be represented as a string that is a permutation of `abcde..`.  For instance, the solution of the 4-queen problem depicted above can be represented by `bdac`.

A simple algorithm is thus to generate all possible $n!$ permutations, and for each one, check if it is a valid placement.  We already ensure that there is exactly one queen per row and one queen per column.  It remains to check that the queens do not threaten each other diagonally.

Let's write a function that checks, given a string representation of the queen positions, whether there is any queen that threaten another or not:

```C
/**
 * Checks if the queen on curr_row clashes with any queens from 
 * curr_row+1 to end_row, inclusive.
 *
 * @param[in] queens   The string representation of the queens.
 * @param[in] curr_row The row where the queen to check for clashes 
 *                     is on.
 * @param[in] end_row  The last row until which we check for 
 *                     clashes.
 *
 * @pre    0 <= curr_row <= end_row
 * @return true if there is a queen that clahes with queen[curr_row] 
 *         diagonally, false otherwise.
 */
bool has_a_queen_in_diagonal(const char queens[], size_t curr_row, 
                             size_t end_row) {
  char curr_col = queens[curr_row];
  char left_col = curr_col - 1;
  char right_col = curr_col + 1;
  for (size_t row = curr_row+1; row <= end_row; row += 1) {
    if (queens[row] == left_col || queens[row] == right_col) {
      return true;
    }
    left_col -= 1;
    right_col += 1;
  }
  return false;
}

/**
 * Checks if any queen from row 0 to end_row (inclusive) 
 * that clashes with each other, diagonally.
 *
 * @param[in] queens   The array containing the representation 
 *                     of the queens.
 * @param[in] end_row  The last row until which we check for 
 *                     clashes.
 *
 * @pre    0 <= end_row <= n-1
 * @return true if there are two queens that clash with each other.
 */
bool threaten_each_other_diagonally(char queens[], size_t end_row) {
  for (size_t begin_row = 0; begin_row <= end_row; begin_row += 1) {
    if (has_a_queen_in_diagonal(queens, begin_row, end_row)) {
      return true;
    }
  }
  return false;
}
```

Solving the $n$ queens problem is then easy.  The code below prints out all possible solutions to the $n$ queens problem.

```C
/**
 * Search for all possible queens placement from row to n-1, 
 * given the queens placement from row 0 to row-1.
 *
 * @param[in] queens  The string representation of queens 
 *                    placement.
 * @param[in] n       The size of the chess board
 * @param[in] row     The last row where the queens positions 
 *                    have been fixed.
 */
void nqueens(char queens[], size_t n, size_t row) {
  if (row == n - 1) {
    if (!threaten_each_other_diagonally(queens, row)) {
      cs1010_println_string(queens);
    }
    return;
  }

  for (size_t i = row; i < n; i++) {
    swap(queens, row, i);
    nqueens(queens, n, row + 1);
    swap(queens, row, i);
  }
}
```

### Making It Faster: Pruning Impossible Solutions

One of the principles of writing efficient code is to avoid doing useless work.  The code above, which tests all permutations, actually generates much useless work.  Suppose the queens in the first two rows already threaten each other, then, there is no need to continue to generate all possible placements of queens for the remaining rows.

This concept is called _pruning_ and is a key to speeding up many searching-based solutions.  _We want to prune away bad solutions as early as possible_.  This can be achieved easily, by checking if the queens already placed on the first $k$ rows threaten each other.

```C
/**
 * Search for all possible queens placement from row to n-1, 
 * given the queens placement from row 0 to row-1.
 *
 * @param[in] queens  The string representation of queens 
 *                    placement.
 * @param[in] n       The size of the chess board
 * @param[in] row     The last row where the queens positions 
 *                    have been fixed.
 */
void nqueens(char queens[], long n, long row) {
  if (row == n - 1) {
    if (!threaten_each_other_diagonally(queens, n - 1)) {
      cs1010_println_string(queens);
    }
    return;
  }

  for (long i = row; i < n; i++) {
    swap(queens, row, i);
	if (!threaten_each_other_diagonally(queens, row)) {
      nqueens(queens, n, row + 1);
	}
    swap(queens, row, i);
  }
}
```

Adding this condition can speed up the code significantly.

Such an algorithm is also known as a _branch-and-bound_ algorithm, where we attempt different possible solutions and prune away partial solutions that we know will never work.  This is an important concept in artificial intelligence (AI)  In fact, the n-queen problem is a typical example used in introductory AI courses for branch-and-bound search.  Again, such solutions are more naturally expressed recursively.

## Problem Set 26

### Problem 26.1

In the code with pruning above, we check if the queens placed on Rows 0 to `row` threaten each other, and call `nqueens` recursively only if these queens do not threaten each other.  Identify the repetitive work being done in the calls `threaten_each_other_diagonally`, and suggest a way to remove the repetitive work.

### Problem 26.2

Consider the code to generate all possible permutations of a string from Problem 25.1.  Suppose that we restrict the permutations to those where the same character does not appear next to each other.  Modify the solution to Problem 25.1 to prune away permutations where the same character appears more than once consecutively.

## Appendix: Complete Code

```C
include "cs1010.h"
#include <stdbool.h>
#include <stdio.h>
#include <string.h>
#include <unistd.h>

/**
 * Draw the chessboard on screen.
 *
 * @param[in] queens The string representation of the queens
 * @param[in] n      The size of the chessboard
 */
void draw(char queens[], size_t n) {
  cs1010_clear_screen();
  for (size_t i = 0; i < n; i += 1) {
    for (char col = 'a'; col < 'a' + (char)n; col += 1) {
      if (queens[i] == col) {
        putchar('#');
      } else {
        putchar('.');
      }
    }
    putchar('\n');
  }
  usleep(10000);
}

void swap(char a[], size_t i, size_t j) {
  char temp = a[i];
  a[i] = a[j];
  a[j] = temp;
}

/**
 * Checks if the queen on curr_row clashes with any queens
 * from curr_row+1 to end_row, inclusive.
 *
 * @param[in] queens   The string representation of the queens.
 * @param[in] curr_row The row where the queen to check for
 *                     clashes is on.
 * @param[in] end_row  The last row until which we check for
 *                     clashes.
 *
 * @pre    0 <= curr_row <= end_row
 * @return true if there is a queen that clahes with queen[curr_row]
 *         diagonally false otherwise.
 */
bool has_a_queen_in_diagonal(const char queens[], size_t curr_row,
                             size_t end_row) {
  char curr_col = queens[curr_row];
  char left_col = curr_col - 1;
  char right_col = curr_col + 1;
  for (size_t row = curr_row+1; row <= end_row; row += 1) {
    if (queens[row] == left_col || queens[row] == right_col) {
      return true;
    }
    left_col -= 1;
    right_col += 1;
  }
  return false;
}

/**
 * Checks if any queen from row 0 to end_row (inclusive) that
 * clashes with each other, diagonally.
 *
 * @param[in] queens   The array containing the representation
 *                     of the queens.
 * @param[in] end_row  The last row until which we check for clashes.
 *
 * @pre    0 <= end_row <= n-1
 * @return true if there are two queens that clash with each other.
 */
bool threaten_each_other_diagonally(char queens[], size_t end_row) {
  for (size_t begin_row = 0; begin_row <= end_row; begin_row += 1) {
    if (has_a_queen_in_diagonal(queens, begin_row, end_row)) {
      return true;
    }
  }
  return false;
}

/**
 * Fix a[0]..a[k - 1] but permute queens a[k]..a[len - 1]
 * Print out each permutation if valid.
 *
 * @param[in,out] a The array to permute
 * @param[in]     n The size of the array
 * @param[in]     k The starting index at which we will permute
 *
 * @post The string a remains unchanged
 */
bool nqueens(char a[], size_t n, size_t k) {
  if (k == n-1) {
    draw(a, n);
    if (!threaten_each_other_diagonally(a, n - 1)) {
      cs1010_println_string(a);
      return true;
    }
    return false;
  }
  for (size_t i = k; i < n; i+= 1) {
    swap(a, k, i);
    if (!threaten_each_other_diagonally(a, k)) {
      if (nqueens(a, n, k+1)) {
        return true;
      }
    }
    swap(a, i, k);
  }
  return false;
}

int main() {
  size_t n = cs1010_read_size_t();
  char *queens = malloc((n + 1) * sizeof(char));
  queens[0] = 'a';
  for (size_t i = 1; i < n; i += 1) {
    queens[i] = queens[i-1] + 1;
  }
  queens[n] = '\0';
  nqueens(queens, n, 0);
  free(queens);
}
```
