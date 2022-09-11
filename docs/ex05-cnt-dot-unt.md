# Exercise 5: Counter, Dot, Unit

### Deadline

This is an ungraded exercise.

### Prerequisite

- Completed [Exercise 3](ex03.md).

### Learning Objectives

- Be comfortable writing C programs that involve fixed-length arrays.
- Be able to write C programs that are neat and readable, adhering to a common prescribed coding convention.

### Grading

This is an ungraded exercise.

## Question 1: Counter

Write a program called `counter` that reads in a non-negative integer and count how many times each digit appears in this number.  Print each digit and the number of times it appears on a separate line, in ascending order of the digits.  Digits that do not appear in the input number need not be printed.

### Sample Runs
```
ooiwt@pe113:~/ex05-ooiwt$ ./counter
24680
0: 1
2: 1
4: 1
6: 1
8: 1
ooiwt@pe113:~/ex05-ooiwt$ ./counter
0
0: 1
ooiwt@pe113:~/ex05-ooiwt$ ./counter
4611686018427387904 
0: 2
1: 3
2: 1
3: 1
4: 3
6: 3
7: 2
8: 3
9: 1
```

## Question 2: Dot

Write a program called `dot` that, given two 4-dimension vectors, find its dot product.

Your program should read the two vectors from the standard inputs.  Each vector is specified by four integers.  Print, to the standard output, the dot product of the two vectors.

### Sample Runs

```
ooiwt@pe113:~/ex05-ooiwt$ ./dot
1 2 3 2
2 1 0 3
10
ooiwt@pe113:~/ex05-ooiwt$ ./dot
100 100 100 100
1 2 -2 -1
0
```

## Question 3: Unit

A unit vector is a vector with a length of 1.  Write a program that, given a 3-dimensional vector, find the unit vector in the same direction.

Your program should read a 3-D vector V from the standard inputs, specified by three integers.  Print, to the standard output, the unit vector in the same direction of V.  Print each element in the vector on a new line.

Solve this problem by writing a function with the following header.

```C
void find_unit_vector(const long v[3], double unit[3]) { .. }
```

### Sample Runs
```
ooiwt@pe113:~/ex05-ooiwt$ ./unit
1 1 1
0.5774
0.5774
0.5774
ooiwt@pe113:~/ex05-ooiwt$ ./unit
-2 0 1
-0.8944
0.0000
0.4472
```
