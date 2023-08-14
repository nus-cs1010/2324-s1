# Assignment 6: Sort, Valley, Lexicon

### Deadline

25 October 2022 (Tuesday), 23:59 PM

### Prerequisite

- Understand how sorting and searching works
- Able to analyze the running time of an algorithm

### Learning Objectives

- Able to exploit underlying properties of data to design efficient algorithms
- Able to select appropriate sorting algorithms to solve a given problem

### Grading

This assignment contributes to **4.0%** of your final grade.  The total marks for this assignment are 40 marks.  

We may deduct up to 1 mark _for each question_ for style violation.  Ensure that your code is neat and readable, adhering to the CS1010 coding convention.

Furthermore, 2 mark is allocated to documentation. Your code should follow the good practices of breaking down your solution into small functions. Document each function (except `main`) following the Doxygen documentation format, as outlined [here](../guides/documentation.md).  Your code must have at least one non-trivial function (other than `main()`) to be eligible for these 2 marks.

{++NEW++}: Each question has an efficiency requirement.  You need to solve the problem within the given running time to get the efficiency marks.  _We may deduct marks if your code is inefficient (e.g., perform redundant or duplicate work), even if its running time is within the given bound_.

The rest of the marks are allocated to correctness -- this includes the correctness of syntax, practices, approach, and logic, and whether you correctly follow our instructions.  Note that _even if your solution produces the correct output every time, it may not get full marks if the approach is wrong._


## Question 1: Sort (12 marks)

A v-array is an array of integers with a special property.  The array can be partitioned into two segments: in the first segment, the numbers are sorted in non-ascending order.  In the second part, the numbers are sorted in non-descending order.  For example, `9 4 2 5 5 8` is a v-array.  `9 4` is in non-ascending order, while `2 5 5 8` are in non-descending order.   On the other hand, `9 4 5 2 5 8` is not a v-array.  

A v-array can be only non-ascending or non-descending.  For instance, both `1 2 3`, `8 8 8 8`, and `-2 -5 -10` are all valid v-arrays.

Write a program `sort`, that reads from standard input:

- a positive integer $n$,
- followed by $n$ numbers that form a v-array.

The program `sort` then sorts the numbers in the v-array in non-descending order and prints the numbers to the standard output, one number per line.

### Grading Criteria

| Criteria | Marks |
|--------------|---|
|Documentation | 2 |
|Efficiency    | 5 |
|Correctness   | 5 |

You must solve this problem with an $O(n)$ algorithm.

Solving this in $O(n^2)$ or $O(n \log{n})$ is trivial, and you will not receive any efficiency marks.

### Sample Runs

```
ooiwt@pe119:~/as06-skeleton$ cat inputs/sort.1.in
5
5 4 2 1 3
ooiwt@pe119:~/as05-skeleton$ ./sort < inputs/sort.1.in
1
2
3
4
5
```

## Question 2: Valley (12 marks)

A _strict v-array_ is a v-array where no two adjacent elements have the same value.

In this problem, we wish to find the minimum value of a strict v-array, known as a _valley_.  

Write a program `valley`, that reads from standard input:

- a positive integer $n$,
- followed by $n$ numbers that form a strict v-array.

The program `valley` then prints the valley of the array to the standard output.

### Grading Criteria

| Criteria | Marks |
|---------------|---|
| Documentation | 2 |
| Efficiency    | 5 |
| Correctness   | 5 |

You must solve this problem with an $O(\log{n})$ algorithm.

Solving this in $O(n)$ is trivial, and you will not receive any efficiency marks.

### Sample Runs

```
ooiwt@pe119:~/as06-skeleton$ cat inputs/valley.1.in
5
5 4 2 1 3
ooiwt@pe119:~/as05-skeleton$ ./sort < inputs/valley.1.in
1
```

## Question 3: Lexicon (16 Marks)

Given a lexicon consisting of words that are made up of a set of symbols, we wish to sort them in increasing lexicographical order.  Assume that the order of two symbols is defined, the ordering of two words is defined as follows:

- Given two words $W_1 = a_0a_1..a_k$ and $W_2 = b_0b_1..b_k$ of equal length.  Let $i$ be the smallest index where the two words differ, i.e., $a_j = b_j$ for $j = 0, 1, .., i-1$ and $a_i \not = b_i$.  Then $W_1 < W_2$ if $a_i < b_i$.  

- Given two words $W_1 = a_0a_1..a_l$ and $W_2 = b_0b_1..b_k$ of different length, with $l < k$, then the ordering pads $W_1$ with a special symbol that is smaller than every other symbols, until both $W_1$ and $W_2$ are of equal length. 

For this question, we consider the set of printable ASCII characters, except the white space `' '`, as the set of symbols in our lexicon.  The ordering of two symbols is defined by the ordering of the ASCII value of the characters.  We can thus use the null character `'\0', with an ASCII value of 0, as the padding symbols.

For instance, the word `ooi` is smaller than `oops`.  The first symbol that differs is at position 2 (the first letter is position 0) and the ASCII code for `i` is smaller than `p`.

As another example, given `ooi` and `ooink`, the ordering pad `ooi` with the null character at position 3, to give the order `ooi` < `ooink`.

Write a program `lexicon`, that reads from standard input:

- a positive integer $n$,
- followed by $n$ words 

Print, to the standard output, 

- the list of words in lexicographical order, one word per line.

### Grading Criteria

| Criteria     | Marks |
|--------------|---|
|Documentation | 2 |
|Efficiency    | 7 |
|Correctness   | 7 |

Suppose that $k$ symbols are used in the lexicon, and the longest string is of length $m$.  You must solve this problem with the running time of $O(m(n + k))$.

Solving this in $O(mn^2)$ or $O(mn \log{n})$ is trivial, and you will not receive any efficiency marks.

### Sample Runs

```
ooiwt@pe119:~/as06-skeleton$ cat inputs/lexicon.1.in
6
c
s
1
0
1
0
ooiwt@pe119:~/as05-skeleton$ ./lexicon < inputs/lexicon.1.in
0
0
1
1
c
s
ooiwt@pe119:~/as06-skeleton$ cat inputs/lexicon.2.in
9
cow
dog
cat
muk
ant
bee
fox
hen
for
ooiwt@pe119:~/as05-skeleton$ ./lexicon < inputs/lexicon.2.in
ant
bee
cat
cow
dog
for
fox
hen
muk
ooiwt@pe119:~/as06-skeleton$ cat inputs/lexicon.3.in
3
ooi
oops
ooink
ooiwt@pe119:~/as05-skeleton$ ./lexicon < inputs/lexicon.3.in
ooi
ooink
oops
```
