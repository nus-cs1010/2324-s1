# Exercise 9: List, Length, Concat, Search

### Deadline

This is an ungraded exercise.

### Prerequisite

- Completed [Exercise 4](ex04-oni-bin-fib.md)

### Learning Objectives

- Learn how to use dynamically-allocated array in C
- Processing and manipulating strings in C.  

### Constraints

Solve these problems without using any C library functions from `string.h`.  Your program must not assume any limit to the length of the input strings (except that their length can fit into the type `size_t`).

### Grading

This is an ungraded exercise.

## Question 1: List

Write a program `list` that reads a list of integers from the standard input, stores it in a dynamically-allocated array, and then prints out the list in reverse order, one number per line.

The first integer read from the standard input is $k$ ($k > 0$), the number of elements of the list.  The next $k$ integers are the elements of the list.

### Sample Run

```
ooiwt@pe101:~$ ./list
3
100 -120 130
130
-120
100
```

## Question 2: Length

Write a program `length` that reads a line of text from the standard input and print the number of characters in the line (including the newline character) to the standard output.

You may use [`cs1010_read_line`](../guides/library.md#char-cs1010_read_line) to read the line of text from the standard input.

Fill in the following method in the given skeleton code:
```
size_t length_of(const char *str) {
	// fill in the function
}
```

The goal of this exercise is to let you practice processing and manipulating strings in C.  Solve this problem directly without using any C library functions from `string.h`.

### Sample Run

```
ooiwt@pe101:~$ ./length
Hello world!
13
```


## Question 3: Concat

Write a program `concat` that reads two words from the standard input, append them into a new string, and then print a new string, to the standard output.

You may use [`cs1010_read_word`](../guides/library.md#char-cs1010_read_word) to read a word from the standard input.

Fill in the following method in the given skeleton code:
```
char* concatenate(const char *str1, const char *str2) {
	// fill in the function
}
```

THe function is responsible for allocating the required amount of memory for the resulting string.  The caller of `concatenate` is responsible for deallocation.

The goal of this exercise is to let you practice processing and manipulating strings in C.  Solve this problem directly without using any C library functions from `string.h`.  You may use the method `length_of` you have written above. 

### Sample Run

```
ooiwt@pe101:~$ ./concat
cs
1010
cs1010
```


## Question 4: Search

Write a program `search` that find a list of words $w_0, w_1, .. w_{k-1}$ appear in a given string $s$.

The program reads the following from the standard input:

- The first line of the input is the string $s$
- The next line is an integer $k$
- The next $k$ lines contains one word each.

The program then prints $k$ lines to the standard output:

- If $w_i$ is a substring of $s$, then print the position of the first occurrence of $w_i$ within $s$.  The first character in $s$ has position 0, the second character has position 1, etc.  
- If $w_i$ is not a substring of $s$, then print the string "not found".

You may use [`cs1010_read_word_array`](../guides/library.md#char-cs1010_read_word_array) to read the list of words $w_0..w_{k-1}$ from the standard input.

### Sample Runs

```
ooiwt@pe101:~$ ./search
haystack
1
needle
not found
ooiwt@pe101:~$ ./search
haystack 
2
hay
stack
0
3
ooiwt@pe101:~$ ./search
When life gives you lemons, make lemonade.
4
life
lemon
,
lime
5
20
26
not found
```
