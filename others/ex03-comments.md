# Exercise 3: Three, Factor, Parity, Nine, HDB

## Guide

## Question 1: Three

Write a program called `three` that reads in two integers $x$ and $y$ from the standard input, and prints to the standard outputs, all multiples of three between $x$ and $y$ (inclusive).

## Guide to Question 1:

The question is not very clear about the meaning of "between $x$ and $y$".  Here, we assume this means from $x$ to $y$.  More careful students would set a variable `start` to the smaller of $x$ and $y$, and `end` to be the larger of $x$ and $y$.  Then scan through from `start` to `end` instead.  The rest of the logic follows below.

### Solution A:
The idea here is we want to scan through the numbers from $x$ to $y$, inclusive, and print out that number if it is a multiple of three. Let's first think about the questions to build a loop.

- Q: What is it we want to do repeatedly?  A: We want to print a number $n$ if it is a multiple of three.
- Q: What is the initial state?  A: The number $n$ is $x$
- Q: When do we stop?  A: The number $n$ is more than $y$
- Q: What do we update in each loop?  A: Increment $n$ by 1

We can now translate this into a `for` loop 

```C
void print_threes(long x, long y) {
  for (long n = x; n <= y; n += 1) {
    if (n % 3 == 0) {
      cs1010_println_long(n);
    }
  }
}
```

or a `while` loop:

```C
void print_threes(long x, long y) {
  long n = x;
  while (n <= y) {
    if (n % 3 == 0) {
      cs1010_println_long(n);
    }
    n += 1;
  }
}
```

We can even do this with a `do-while` loop.  If there is always at least one number to check, we will always enter the loop at least once.

```C
void print_threes(long x, long y) {
  long n = x;
  do {
    if (n % 3 == 0) {
      cs1010_println_long(n);
    }
  n += 1;
  }
  while (n <= y);
}
```

### Solution B:

This is not the only solution.  Let's look at another.  Suppose we answer the questions differently:

- Q: What is it we want to do repeatedly?  A: We want to print a number $n$
- Q: What is the initial state?  A: The number $n$ is the smallest multiple of three bigger or equals to $x$
- Q: When do we stop?  A: The number $n$ is more than $y$
- Q: What do we update in each loop?  A: Increment $n$ by 3

Then we can end up with very different code:

```C
long next_mulitple_of_three(long x) {
  if (x % 3 == 0) {
    return x;
  }
  if (x > 0) {
    return x + (3 - (x % 3));
  }
  return x + (-x % 3);
}

void print_threes(long x, long y) {
  x = next_mulitple_of_three(x);  
  for (long n = x; n <= y; n += 3) {
    cs1010_println_long(n);
  }
}
```

## Question 2: Factor

Given a number $n$, we want to find out how many factors $n$ has, excluding the trivial factor 1 and $n$.

Write a program `factor` that reads, from the standard input, a positive integer $n$, and prints, to the standard output, the factors of $n$ between 2 and $n-1$, inclusive.

## Guide to Question 2:

The idea here is we want to scan through the numbers from $2$ to $n-1$, inclusive, and count how many factors are there.  It should be clear that we need to introduce a new variable to keep track of the number of factors.  Let's call this variable `num_of_factors`.

- Q: What is it we want to do repeatedly?  A: We want to increment `num_of_factors` if the current number $i$ is a factor of $n$.
- Q: What is the initial state?  A: $i$ is 2 and `num_of_factors` is 0.
- Q: When do we stop?  A: When $i$ is bigger than $n-1$.
- Q: What do we update in each loop?  A: Increment $i$ by 1.

We can now translate this into a `for` loop 

```C
long count_factors(long n) {
  long num_of_factors = 0;
  for (long i = 2; i <= n - 1; n += 1) {
    if (n % i == 0) {
      num_of_factors += 1;
    }
  }
  return num_of_factors;
}
```

or a `while` loop:

```C
long count_factors(long n) {
  long i = 2;
  long num_of_factors = 0;
  while (i <= n - 1) {
    if (n % i == 0) {
      num_of_factors += 1;
    }
  i += 1;
  }
  return num_of_factors;
}
```

We can also do this with a `do-while` loop, but since a `do-while` loop loops at least once, we need to make sure to handle the special case where there is nothing to loop through (i.e., when $n$ is 2).

```C
long count_factors(long n) {
  if (n == 2) {
    return 0;
  }
  long i = 2;
  long num_of_factors = 0;
  do {
    if (n % i == 0) {
      num_of_factors += 1;
    }
  i += 1;
  }
  while (i <= n - 1);
  return num_of_factors;
}
```

An observant student might observe that 3 is prime and so when $n$ is 3, there are no factors either.  So we could equivalently say:

```C
long count_factors(long n) {
  if (n <= 3) {
    return 0;
  }
  long i = 2;
  long num_of_factors = 0;
  do {
    if (n % i == 0) {
      num_of_factors += 1;
    }
  i += 1;
  }
  while (i <= n - 1);
  return num_of_factors;
}
```

## Question 3: Parity

Write a program `parity`, that reads from standard input a positive integer $n$
print, to the standard output, 2 lines,
```
odd: X
even: Y
```
where `X` represents the number of odd digits and `Y` represents the number of even digits.

## Guide to Question 3

Here, we need to loop through every digit in $n$.  Just like your other exercises, you can use `% 10` to retrieve the last digit and `/ 10` to retrieve the remaining digits.  It should also be clear that you need two additional variables, to keep track of the number of odd digits and even digits. 

- Q: What is it we want to do repeatedly?  A: We want to check the last digit.  Increment the odd counter if it is odd; the even counter if it is even.
- Q: What is the initial state?  A: Both counters are set to 0.
- Q: When do we stop?  A: When there is no more digit.
- Q: What do we update in each loop?  A: We remove the last digit from $n$.

Since the initialization involves multiple statements, it is more natural to do it with a `while` loop.

```C
void print_odd_even(long n) {
  long num_evens = 0;
  long num_odds = 0;
  while (n != 0) {
	if ((n % 10) % 2 == 0) {
      num_evens += 1;
    } else {
      num_odds += 1;
    }
    n /= 10;
  }
  cs1010_print_string("odd: ");
  cs1010_println_long(num_odds);
  cs1010_print_string("even: ");
  cs1010_println_long(num_evens);
}
```

You may notice that, since $n$ is positive and is guaranteed to have at least one digit, we are guaranteed to enter the loop at least once.  So, we can also do this with a `do-while` loop.

```C
void print_odd_even(long n) {
  long num_evens = 0;
  long num_odds = 0;
  do {
	if ((n % 10) % 2 == 0) {
	  num_evens += 1;
    } else {
	  num_odds += 1;
    }
    n /= 10;
  }
  while (n != 0);
  cs1010_print_string("odd: ");
  cs1010_println_long(num_odds);
  cs1010_print_string("even: ");
  cs1010_println_long(num_evens);
}
```

Food for thought: if we allow 0 as a valid input, which version is correct?

## Question 4: Nine

Write a program that looks for the least significant occurrence of digit 9 in a given number.

Your program, `nine`, should read a positive number from the standard input and print out the position of the least significant occurrence of 9.  The rightmost digit has the position of 1, the second last has the position of 2, etc.  If the number 9 does not appear in the given number, print 0.

## Guide to Question 4

This question has a slightly more complex stopping condition.  The idea is that we wish to scan through the digits, looking for a 9.  We need to keep track of the position, so we need an additional variable `position`.

- Q: What is it we want to do repeatedly?  A: We want to check the last digit if it is a `9`. 
- Q: What is the initial state?  A: `position` is set to 1.
- Q: When do we stop?  A: When there is no more digit or when we find a `9`.
- Q: What do we update in each loop?  A: We increment `position` by 1, and remove one digit from the number.

### Method A
The first method requires us to keep a boolean variable to keep track of whether we have found a 9.  We need to augment the initialization so that we also set this variable (let's call it `found9`) to `false`.

- Q: What is the initial state?  A: `position` is set to 0, `found9` is set to `false`

Here is the `while` version:

```C
long find_last_significant_9(long n) {
  long position = 1;
  bool found9 = false;
  while (n != 0 && !found9) {
    if (n % 10 == 9) {
      found9 = true;
    } 
    position += 1;
    n /= 10;
  }
  if (!found9) {
    return 0;
  }
  return position - 1;
}
```

What we do after we exit the loop is a bit tricky.  On Line 11, we can assert that { `found9 == true || n == 0` } (applying De Morgan's Law to the loop condition).  So, either one of this is true: `found9` is `true`, or `found9` is `false` and `n == 0`.  We have to handle the two cases separately.  If `found9` is false, then we need to return 0 as per the requirement.  If `found9` is true, then we need to remove one less than the position (since we increment it on Line 8 after we found a 9.

### Method B

The second method is simpler.  Take a look:

```C
long find_last_significant_9(long n) {
  long position = 1;
  while (n != 0) {
  if (n % 10 == 9) {
    return position;
  } 
  position += 1;
  n /= 10;
  }
  return 0;
}
```

As soon as we found a 9, we can return from the function on Line 5.  If we exit from the `while` loop without returning, then we are guaranteed that we have not found a 9.  Note that the assertion immediately after the while loop is { `n == 0` }, which is much simpler.  So, we can simply return 0 on Line 10.

## Question 5: HDB

ASCII Art refers to the art of drawing with only common letters, numbers, and symbols on our keyboard.  My daughter has discovered that, if we draw rows of
`#` symbols together, it approximately looks like an HDB flat!

Write a program `hdb` that takes in two positive integers $w$ and $h$, and draw $h$ rows of `#` symbols, each row containing $w$ `#`, with no spaces before, in between, and after.

## Guide to Question 5

This question requires two loops: one to draw each individual floor, the other to draw the whole block.  The loop itself, however, is simple.  I think we can skip answering the four loop questions for this problem:

### Solution A

```C
void print_floor(long w) {
  long i;
  for (i = 0; i < w - 1; i += 1) {
    cs1010_print_string("#");
  }
  cs1010_println_string("#");
}

void print_hdb(long w, long h) {
  long i;
  for (i = 0; i < h; i += 1) {
  print_floor(w);
  }
}
```

### Solution B

Some students have asked about nested loops.  You can solve this problem with one loop nested within another.  We have not shown you this in class before, but it is not more complicated than nesting a conditional within a loop.

A nested loop is more prone to bugs since we need to keep track of two loop variables in the same scope and we are doing two things within the same function.  It is easier to get mixed up. 

See if you can catch what is wrong with the code below:

```C
// Incorrect
void print_hdb(long w, long h) { 
  long i;
  for (i = 0; i < h; i += 1) {
    for (i = 0; i < w - 1; i += 1) {
      cs1010_print_string("#");
    }
    cs1010_println_string("#");
  }
}
```

```C
// Incorrect
void print_hdb(long w, long h) { // buggy
  long i;
  long j;
  for (i = 0; i < h; i += 1) {
    for (j = 0; j < w - 1; j += 1) {
      cs1010_print_string("#");
    }
  }
  cs1010_println_string("#");
}
```
