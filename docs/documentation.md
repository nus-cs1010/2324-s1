# Code Documentation

Code documentation is as important as the code itself.  It helps readers of your code, including your future self, to understand

-  the purpose of a piece of code
-  what assumptions are being made, and
-  the reasoning behind why certain things are done.

## C Syntax for Comments

In C, you can write comments in two ways:

- Either prefix a one-line comment with two slashes `//` , or
- Write multiple-line comments between `/*` and `*/`

For example:

```C
// assume the number of elements > 1
```

```C
/*
 This function reads in the radius of a sphere and returns the
 volume of the sphere.  We assume the radius is normalized between
 0.0 and 1.0.
 */
```

## The Doxygen Format

In CS1010, we will adopt the Doxygen format for C comments.  Doxygen is a tool that automatically generates HTML documents from comments in C code and is widely used in the industry.

We write a Doxygen comment with an additional `*` after `/*`:

```C
/**

 */
```

The comments can be free-form text.  However, to help with creating a more structured document, we can add what Doxygen calls special "commands".  I view these commands as keys to certain information.  Useful commands are:

### `@author`: The name the author

This is used to identify the author and placed at the top of the `.c.` file   This is what you have been using since Exercise 0.

### `@file`: The name of a file

This is used to identify the name of the file and be placed at the top of the `.c.` file.  This is usually written for you already.

### `@pre`: The precondition of a function

If your function makes certain assumptions about the inputs, explain it using this command. This is used to document assertions at the beginning of your function (e.g., a string is not empty, a pointer is not NULL, etc)

### `@post`: The postcondition of a function

This command is used to document assertions that are true just before you return from your function.

### `@param[<direction>] <name>`: Describe a parameter of a function.  

`<name>` is the name of the parameter (can be a variable, array, pointers, struct, etc).

`<direction>` indicates if you are passing data in or out of the function.  From Unit 1 to 16, we only pass data _into_ the function.  For such parameters, we document it with `@param[in]`.  In Unit 17, we will learn about passing by reference.  If a parameter is passed by reference to be modified inside the function, we will document it as `@param[out]`.  For a parameter that is meant to serve both purposes (pass a value into the function, be modified, and passed the new modified value out), we use `@param[in,out]`.

### `@return`: Describe the return value of a function

The comments should be placed before a file, a function, or a variable that you want the comment to apply to.

## Example 1: CS1010 I/O Library

```C
/**
 * @file: cs1010.c
 * @author: Ooi Wei Tsang
 *
 * This file contains the implementation of the CS1010 I/O library to
 * simplify the reading and writing of integer, real numbers, and text
 * from the standard input and output respectively.
 */

/**
 * Read k white-space-separated words from the standard input in an array.
 * The notion of "word" is the same to cs1010_read_word().  The caller is
 * responsible for freeing the memory allocated for the array by calling
 * free().
 *
 * @param[in] k The number of words to read.
 * @return Returns NULL if there is a memory allocation error, otherwise,
 * return an array of char* containing the words.
 */
char** cs1010_read_word_array(size_t k)
{
   :
}
```

## Example 2: Collatz

```C
/**
 * Find the number between 1 and n with the maximum number of 
 * steps to reach 1, breaking ties by finding the larger among
 * these numbers.
 *
 * @param[in,out] max_num_steps The maximum number ot steps
 * @param[out] max_n The number with the maximum number of steps
 *
 * @pre max_num_steps <= 0
 */
void solve(long n, long *max_n, long *max_num_steps) {
  for (long i = 1; i <= n; i += 1) {
    long num_of_steps = count_num_of_steps(i);
    if (num_of_steps >= *max_num_steps) {
      *max_n = i;
      *max_num_steps = num_of_steps;
    }
  }
}
```

Note: a better design of the function above is not to rely on the 
caller to set `max_num_steps` to 0, but set it at the beginning of 
the function ourselves.

## Example 3: Taxi

```C
/**
 * Checks if a given day is a weekday or not.
 *
 * @param[in] day This indicates which day it is.  1 is Monday; 
 *                7 is Sunday.
 * @return Returns true if day is a weekday, false otherwise.
 *
 * @pre 1 <= day <= 7
 */
bool is_weekday(long day)
{
    return (day >= 1 && day <= 5);
}
```
