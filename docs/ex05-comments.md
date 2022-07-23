Exercise 5: Counter, Dot, Unit
------------------------------

Question 1: Counter
-------------------

Write a program called `counter` that reads in a non-negative
integer and count how many times each digit appears in this
number.  Print each digit and the number of times it appears
on a separate line, in ascending order of the digits.  Digits
that do not appear in the input number need not be printed.

# Guide

## Using a Loop

One way to solve this is to count, for each digit, how many times it has appear in the given number.  The function `count_digit` count how many times digit `d` appears in `n`.

```C
void print_digit_count(long digit, long count) {
  if (count != 0) {
    cs1010_print_long(digit);
    cs1010_print_string(": ");
    cs1010_println_long(count);
  }
}

long count_digit(long n, long d) {
  if (n == 0 && d == 0) {
    return 1;
  }
  long counter = 0;
  while (n != 0) {
    if (n % 10 == d) {
      counter += 1;
    }
    n = n / 10;
  }
  return counter;
}

int main()
{
  long n = cs1010_read_long();

  for (long digit = 0; digit < 10; digit += 1) {
    long counter = count_digit(n, digit);
	print_digit_count(digit, counter);
  }
}
```

## Using an Array

We can make the code more efficient.  To scan through the input once and count each digit, we would need 10 counters.  Instead of having variables `counter0`, `counter1`, `counter2`, etc.  We could use an array of 10 counters.  Then, we only need to scan through the number once.

```C
void count_digits(long n, long counter[10]) {
  if (n == 0) {
    counter[0] = 1;
    return;
  }
  while (n != 0) {
    counter[n % 10] += 1;
    n = n / 10;
  }
}
```

The `main` function then looks like this:

```C
int main()
{
  long counter[10] = {0};

  long n = cs1010_read_long();
  count_digits(n, counter);

  for (long digit = 0; digit < 10; digit += 1) {
    print_digit_count(digit, counter[digit]);
  }
}
```

Question 2: Dot
---------------

Write a program called `dot` that, given two 4-dimension vectors,
find its dot product.

Your program should read the two vectors from the standard inputs.
Each vector is specified by four integers.  Print, to the standard
output, the dot product of the two vectors.


Guide
-----

The code for dot product should be quite straightforward:

```C
long dot_product(long v1[4], long v2[4]) {
  long result = 0;
  for (long i = 0; i < 4; i += 1) {
    result += (v1[i] * v2[i]);
  }
  return result;
}
```

The `main` is quite simple as well.  The only new thing here is that you need to loop through the input to read the vectors.

```C
int main()
{
  long v1[4];
  for (long i = 0; i < 4; i += 1) {
    v1[i] = cs1010_read_long();
  }

  long v2[4];
  for (long i = 0; i < 4; i += 1) {
    v2[i] = cs1010_read_long();
  }

  cs1010_println_long(dot_product(v1, v2));
}
```

Question 3: Unit
----------------

A unit vector is a vector with a length of 1.  Write a program that, given a 3-dimensional vector, find the unit vector in the same direction.

Your program should read a 3-D vector V from the standard inputs, specified by three integers.  Print, to the standard output, the unit vector in the same direction of V.  Print each element in the vector on a new line.

Solve this problem by writing a function with the following header.

```C
void find_unit_vector(const long v[3], double unit[3]) { .. }
```

Guide
------

Note that `v` is the input vector since it is a vector of `long` and has qualifier `const`.  We need to find the magnitude of this vector.  The resulting unit vector can then be computed by dividing `v` with the magnitude.

```C
void find_unit_vector(const long v[3], double unit[3]) {
  double distance = sqrt((v[0] * v[0]) + (v[1] * v[1]) + (v[2] * v[2]));

  for (long i = 0; i < 3; i += 1) {
    unit[i] = v[i] / distance;
  }
}
```
