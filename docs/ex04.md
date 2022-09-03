# Exercise 4: Binary, Onigiri, Fibonacci

### Deadline

This is an ungraded exercise.

### Prerequisite

- Completed [Exercise 3](ex03.md).

### Learning Objectives

- Be comfortable writing C programs that involve loops with while/for/do-while statements.
- Be able to write C programs that are neat and readable, adhering to a common prescribed coding convention.

### Grading

This is an ungraded exercise.

## Question 1: Binary

In this question, you are asked to convert a number represented in binary format (using digits 0 and 1) into decimal format (using digits 0 and 9).

A number in decimal format is represented with base 10.  The last digit (rightmost) corresponds to the unit of 1, the next digit (second last) corresponds to the unit of 10, and so on. So, one can write the decimal number,
for instance, 7146 as 7×1000 + 1×100 + 4×10 + 6×1.

A number represented in binary uses base 2 instead of base 10.  The last digit corresponds to 1 . The second last digit corresponds to 2, the third last digit corresponds to 4, and so on. So, the binary number 1101, for instance, corresponds to 1×8 + 1×4 + 1×1 = 13.

Write a program called `binary` that reads in a positive integer consisting of only 0s and 1s from the standard input, treats it as a binary number, and prints the corresponding decimal number to the standard output.

Try to solve this question with two different approaches: (i) recursively and (ii) iteratively with loops.

### Sample Run

```
ooiwt@pe113:~/ex04-ooiwt$ ./binary
1101
13
ooiwt@pe113:~/ex04-ooiwt$ ./binary
111
7
ooiwt@pe113:~/ex04-ooiwt$ ./binary
10110100
180
```

![onigiri](figures/ex04-onigiri.jpg){ align=left }

## Question 2: Onigiri
Onigiri is a Japanese rice ball often formed in a triangular or cylindrical shape and wrapped in nori. 

We are going to draw some triangular onigiri on our screen using #s.

Write a program `onigiri.c` that draws an isosceles triangle using ` ` (white space) and `#`.  The program must read in a positive integer representing the height $h$ of a triangle.  The triangle must have exactly $h$ rows.  Each row must have exactly $2h-1$ characters (including white spaces but excluding a new line).  On each row, the sequence of "#" characters must be centralized, padded by white spaces on both sides.

### Sample Run

```
ooiwt@pe112:~/ex04-ooiwt$ ./onigiri
5
    #     
   ###    
  #####   
 ####### 
#########
ooiwt@pe112:~/ex04-ooiwt$ ./onigiri
3
  #  
 ### 
#####
ooiwt@pe112:~/ex04-ooiwt$ ./onigiri
1
#
```


### Hints

1.  First, find the pattern to draw the triangle.

    On row $k$, how many `#`s should you draw?  How many white spaces should be padded on the left and the right?

2.  Write a function that draws a particular row of the triangle.  Then, call this function repeatedly in a loop.

3.  White spaces are not visible.  To help you debug, you can pipe the output through a Unix tool called `sed`.

```
ooiwt@pe112:~/ex04-ooiwt$ ./onigiri | sed 's/ /./g'
4
...#...
..###..
.#####.
#######
```

## Question 3: Fibonnaci

The Fibonacci sequence is a sequence of numbers 1, 1, 2, 3, 5, 8, 13, ... Fibonacci numbers often appear in mathematics as well as in nature and have many fascinating properties.

The Fibonacci sequence can be constructed as follows. The first Fibonacci number is 1. The second Fibonacci number is also 1. Subsequently, the $i$-th Fibonacci number is computed as the sum of the previous two Fibonacci numbers, the $(i-2)$-th, and the $(i-1)$-th.

Write a program called `fibonacci` that reads a positive integer number $n$ from the standard input, and prints the $n$-th Fibonacci number to the standard output. 

Try to solve this question with two different approaches: (i) recursively and (ii) iteratively with loops.  

Note that when $n$ is large, your recursive approach may take a long time to run, or even crash with the error `Segmentation fault (core dumped)`.  This is expected.

### Sample Run
```
ooiwt@pe113:~/ex04-ooiwt$ ./fibonacci
1
1
ooiwt@pe113:~/ex04-ooiwt$ ./fibonacci
10
55
ooiwt@pe113:~/ex04-ooiwt$ ./fibonacci
83
99194853094755497
```

