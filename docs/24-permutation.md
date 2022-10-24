# Unit 24: Permutations

## Learning Objectives

After taking this unit, students should

- be familiar with using recursion to generate all possible permutations of a given array

## Generating Permutations

We have been using recursions to either _compute_ or _search_ for a solution to a problem.  In this unit, let's look at another useful application of recursion: to _generate all possible_ permutations or combinations of items.

Let's see a simple example of this: generate all permutations of the characters in a string of length $n$.  Let's say we want to permute the string `abcd` ($n = 4$).  We get the following 24 permutations.

```
abcd bacd cbad dbca
abdc badc cbda dbac
acbd bcad cabd dcba
acdb bcda cadb dcab
adcb bdca cdab dacb
adbc bdac cdba dabc
```

For simplicity, we assume that there is no repetition in the string.  We leave the problem where there is repetition as an exercise in Problem 24.1.

## Recursive Formulation

The recursive formulation of this problem is based on the following observation.  If we consider a subset of all permutations that start with the same prefix of length $k$, then their suffixes are permutations of length $n-k$.

Let's say we consider the prefix `a` in the example above.  The permuted strings that start with `a` are:

```
abcd
abdc
abcd
acdb
adcb
adbc
```

Excluding `a`, the strings have suffixes:

```
bcd
bdc
bcd
cdb
dcb
dbc
```

which are the six possible permutations of the string `bcd`.

Let's consider the prefix `bc` in the example above.  The permuted strings that start with `bc` are:

```
bcad
bcda
```

Excluding `bc`, the strings have suffixes:

```
ad
da
```

which are the two possible permutations of the string `ad`.

This observation gives rise to a simple recursive solution:  To generate all possible permutations of a string of length $n$: $a_0a_1..a_{n-1}$, we loop through every character $a_i$ in the string and generate permutations with $a_i$ as the leading character.  To generate permutations with $a_i$ as the leading character, we simply recursively generate all possible permutations of a string of length $n-1$: $a_0..a_{i-1}a_{i+1}..a_{n-1}$.

The trivial case is when we generate the permutation of a string with one character.  There is only one possible permutation.

For example, consider a string length 3, `abc`.

- We start with `a` as the leading character and generate all the permutations of the string `bc`.  We get two permutations `abc` and `acb`.  
- The next character in the input string is `b`.  We now make `b` the leading character and generate all permutations of the string `ac`.  We get `bac` and `bca`.  
- The last character in the input string is `c`.  We similarly generate the permutations `cab` and `cba` by considering `c` as the leading character and permuting `ba`.

Now, let's consider a longer example, say a string of length 5, `abcde`.

- Just like above, we start with `a` as the first character and permute `bcde`.
- As we recursively permute `bcde`, `a` remains part of the prefix.
- To recursively permute `bcde`, we first fix `b` as the leading character (but `b` is the second character in `abcde`) and permute `cde`.  The prefix is `ab`.
- The next step in permuting `bcde` is to consider `c` as the leading character.  The prefix is now `ac`, and we permute the rest of the string `bde`.

As we go deeper into the recursion, the prefix grows longer.  When we reach the depth of $n-1$, there is only one character to permute.  We have completed generating one possible permutation of the string.  We can print out the permutation at this point and continue with the rest of the permutation.

The figure below illustrates the process

![permute](figures/lec10-permute/lec10-permute-crop-pdf-0.png)

## The Code

The idea above is implemented as the following.  The function `permute` takes in the array `a` to permute, the length `len` of the array, and the location `curr`, where the substring `a[curr]` to `a[len - 1]` is what this function will permute.  The function will print out all permutations of `a` where `a[0]` to `a[curr - 1]` are fixed as the prefix and `a[curr]` to `a[len - 1]` are permuted.

```C
/**
 * Fix a[0]..a[curr - 1] but permute characters a[curr]..a[len - 1]
 * Print out each permutation.
 *
 * @param[in,out] a    The array to permute
 * @param[in]     len  The size of the array
 * @param[in]     curr The starting index at which we will permute
 *
 * @post The string a remains unchanged
 */
void permute(char a[], size_t len, size_t curr) {
  if (curr == len - 1) {
    cs1010_println_string(a);
    return;
  }

  for (size_t i = curr; i < len; i += 1) {
    swap(a, curr, i);
    permute(a, len, curr + 1);
    swap(a, i, curr);
  }
}
```

Lines 12-14 above correspond to the base case, where we have reached the end of the string, and there is only one character left to permute.  Since there is only one possible permutation, we only need to print out the permuted string.

Line 19 permutes the remaining string, `a[curr + 1]` to `a[len - 1]`, with character `a[curr]` intact.  Lines 17-21 is a `for` loop that loops through all characters `a[curr + 1]` to `a[len - 1]`, and swaps each one to the position of `a[curr]`, and recursively permutes the string `a[curr + 1]`..`a[len - 1]`.  When we are done, we swap back the original `a[curr]`, this is to ensure that the string remains unchanged after `permute` is called.

## Running Time

How efficient is the function `permute`?

Let the running time of `permute` when given a string of length $n$ be $T(n)$.  Each invocation of `permute` loops through $n$ characters, and for each character, calls `permute` recursively on a string of length $n-1$.  

So, we have:

\[
\begin{align*}
T(n) &= nT(n-1) + n\\
     &= n((n-1)T(n-2) + (n-1)) + n\\
	   &= n(n-1)T(n-2) + n(n-1) + n\\
	   &= n(n-1)((n-2)T(n-3) + n-2) + n(n-1) + n\\
	   &= n(n-1)(n-2)T(n-3) + n(n-1)(n-2) + n(n-1) + n\\
	   &= \frac{n!}{(n-3)!}T(n-3) + \frac{n!}{(n-3)!} + \frac{n!}{(n-2)!} + \frac{n!}{(n-1)!}\\
	   &= ..\\
	   &= \frac{n!}{(n-k)!}T(n-k) + \sum_{i=1}^{k}\frac{n!}{(n-i)!}
\end{align*}
\]

When we reach the base case, we have $T(1) = n$ since we need to print out the string of length $n$.  This happens when $k = n - 1$.  Therefore,

\begin{align*}
T(n) &= \frac{n!}{1!}T(1) + \sum_{i=1}^{n-1}\frac{n!}{(n-i)!}\\
     &= n \cdot n! + \sum_{i=1}^{n-1}\frac{n!}{(n-i)!}\\
     &< n \cdot n! + \sum_{i=1}^{n-1}n!\\
     &= n \cdot n! + (n - 1) \cdot n!\\
\end{align*}

The running time for `permute` is, therefore, $O(n \cdot n!)$.

## Problem Set

### Problem Set 24.1

In the code above, we assume that the string contains distinct characters.  If there are duplicate characters in the string, duplicate permutations will be generated.  For instance, if the input is `aaa`, the code above would print `aaa` six times.

We can fix this by making a small change to the function `permute` above so that it does not generate duplicate permutations.  This can be done by adding a condition (Line A).  Write a boolean function that we can call in Line A to check if we should continue to permute the rest of the string, and therefore avoid generating duplicate permutations when the input string contains duplicate characters.

```C
void permute(char a[], size_t len, size_t curr) {
  if (curr == len-1) {
    cs1010_println_string(a);
    return;
  }

  permute(a, len, curr + 1);
  for (size_t i = curr + 1; i < len; i += 1) {
    if (...) { // Line A
      swap(a, curr, i);
      permute(a, len, curr + 1);
      swap(a, i, curr);
    }
  }
}
```

## Appendix: Complete Code

```C
#include "cs1010.h"
#include <string.h>

void swap(char a[], size_t i, size_t j) {
  char temp = a[i];
  a[i] = a[j];
  a[j] = temp;
}

/**
 * Fix a[0]..a[k - 1] but permute characters a[k]..a[len - 1]
 * Print out each permutation.
 *
 * @param[in,out] a The array to permute
 * @param[in]     n The size of the array
 * @param[in]     k The starting index at which we will permute
 *
 * @post The string a remains unchanged
 */
void permute(char a[], size_t n, size_t k) {
  if (k == n-1) {
    cs1010_println_string(a);
    return;
  }
  for (size_t i = k; i < n; i+= 1) {
    swap(a, k, i);
    permute(a, n, k+1);
    swap(a, i, k);
  }
}

int main() {
  char *str = cs1010_read_word();
  permute(str, strlen(str), 0);
  free(str);
}
```