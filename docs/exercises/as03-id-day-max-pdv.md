# Assignment 3: ID, Days, Max, Padovan

### Deadline

27 September 2022 (Tuesday), 23:59 pm.

### Prerequisite

- Solve [Exercise 5](ex05-cnt-dot-unt.md)

### Learning Objectives

- Be comfortable writing C programs that use fixed-size arrays.
- Be able to use a fixed-size array as a look-up table.

### Grading

This assignment contributes to 3% of your final grade.  The total marks for this assignment are 30 marks.  

For Assignment 3, we may deduct up to 1 mark _for each question_ for style violation.  Ensure that your code is neat and readable, adhering to the CS1010 coding convention.

3 mark is allocated to efficiency for `padovan`.  Avoid repetitively calculating the same sequence of numbers over and over to earn this 3 marks.

The rest of the marks are allocated to correctness -- this includes the correctness of syntax, practices, approach, and logic, and whether you correctly follow our instructions.  Note that _even if your solution produces the correct output every time, it may not get full marks if the approach is wrong._


## Question 1: Days (7 marks)

Write a program called `days` that reads in two integers from the standard input, the first is the month (ranged from 1 to 12, inclusive) and the second is the day (ranged from 1 to 31, inclusive).  The program then prints to the standard output which day of the year it is.  For instance, September 15 is the 258th day of the year.  If the input to `days` is `9 15`, the program should print `258`.

Assume that the year is not a leap year.

Use a fixed-length array and loops to solve this problem instead of conditional statements.  Solutions that use conditional statements (in any form, including `if-else`, `switch`, and the `? :` operator) will receive a 0.

### Sample Runs
```
ooiwt@pe112:~/ex01-ooiwt$ ./days
1 1
1
ooiwt@pe112:~/ex01-ooiwt$ ./days
9 15
258
ooiwt@pe112:~/ex01-ooiwt$ ./days
12 31
365
```

## Question 2: ID (7 marks)

Your NUS student id has a letter at the end. This letter is called a check code and is a form of redundancy check used for detecting errors, especially when your student id is manually entered into a software application.

Your check code is calculated by:

1. Sum up the digits in your student id. Let the sum be N.
2. Divide N by 13, and take the remainder. Let the remainder
   be R.  
3. Look up the table below:

R  | Check Code
---|------------
0  | Y
1  | X
2  | W
3  | U
4  | R
5  | N
6  | M
7  | L
8  | J
9  | H
10 | E
11 | A
12 | B

Write a program `id.c` that reads in an integer containing the digits of a student's id from the standard input. Print out the check code to the standard output.

Each check code can be stored as a `char` variable.  In C, `char` constants are surrounded by a single quote, e.g., `'A'`.  You can use the `putchar` function to print a single `char` variable to the standard output.

Example:
```C
#include "cs1010.h"
int main() {
  char a[2] = { 'A', 'B' }; // an array of two `char`
  putchar(a[0]); // print 'A'
}
```

Use a fixed-length array and loops to solve this problem instead of conditional statements.  Solutions that use conditional statements (including `if-else`, `switch`, and the `? :` operator) will receive a 0.

### Sample Runs
```
ooiwt@pe116:~/as03-ooiwt$ ./id
1933091
Y
ooiwt@pe116:~/as03-ooiwt$ ./id
3364497
E
ooiwt@pe116:~/as03-ooiwt$ ./id
1111111111111
Y
```

### Question 3: Max (8 marks)

Write a program max that finds the maximum value from a list $L$ of $k$ integers.

Instead of doing this with a loop, solve this question with _recursion_.  Fill in the function

```
long find_max(const long list[], long start, long end)
{
	:
}
```

that calls itself recursively and returns the maximum value among the array elements `list[start] .. list[end - 1]`.  

In the function definition above, the keyword `const` (short for constant) is used to annotate that the array list is meant to remain unchanged.

The program should read the following from the standard inputs:

- The first number is a positive integer $k$.
- The next $k$ numbers correspond to the list of integers $L$.

The program should then print the largest integer from the list to the standard output.

$k$ is guaranteed to be no more than 100000.  We can use a fixed-length array on the stack to store the inputs.  The `main` function to read the inputs and print the maximum value has been given.  You should not change the `main` function unless you have a good reason.

Since you are supposed to solve this problem with recursion, you are not allowed to use loops of any kind (`for`, `while`, `do-while`) inside the function `find_max`.  Solutions that use loops inside `find_max` will receive 0.

### Sample Runs

```
ooiwt@pe116:~/as03-ooiwt$ cat input
5
-5 3 1 8 2
ooiwt@pe116:~/as03-ooiwt$ ./max < input
8
```


## Question 4: Padovan (8 marks)

Padovan numbers are a sequence of numbers defined as follows:

$$
p(i)=
\begin{cases}
1 & i = 0 \\
0 & i = 1 \\
0 & i = 2 \\
p(i-2) + p(i-3) & i > 2\\
\end{cases}
$$


You might notice the similarity between the Padovan numbers and Fibonacci numbers.  Padovan numbers are named after [Richard Padovan](https://en.wikipedia.org/wiki/Richard_Padovan), an architect.  Unlike Fibonacci, Padovan is "only" 89 years old and the sequence is named after him only recently!

Write a program `padovan` that reads in a non-negative integer $n$ from the standard input, and writes to the standard output, all the Padovan numbers $p(n)$, $p(n-1)$, .. $p(0)$, in order (one number per line).

$n$ is guaranteed to be 160 or less.

There are 3 marks allocated for efficiency for this question.  Avoid repetitively re-calculating the same sequence of numbers over and over to earn these 3 marks.

### Sample Runs

```
ooiwt@pe116:~/as03-ooiwt$ ./padovan
5
1
0
1
0
0
1
ooiwt@pe116:~/as03-ooiwt$ ./padovan
0
1
```

