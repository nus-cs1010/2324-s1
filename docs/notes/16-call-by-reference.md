# Unit 16: Call by Reference

## Learning Objectives

After completing this unit, students should:

- understand the differences between call-by-value and call-by-reference
- understand the mechanism we can perform call-by-reference in C using stack and pointers
- know the situations where call-by-reference is useful
- be able to read and write code that uses call-by-references
- know how to write Doxygen documentation to document the parameters of a function.

## Call By Reference

In [Unit 14](14-array.md), we have seen how an array is passed by reference into functions.  When we pass an array `a` into a function, due to array decay, we are passing in the pointer to the first element of the array (`&a[0]`).  So, what gets copied onto the call stack is the pointer to the array, not the actual array itself.  Now, with this pointer, we can modify the elements in the array directly.

## Passing Non-Array Variables by References

We mentioned that for all other non-array variables when we pass in a variable, the value of the variable gets copied onto the stack.  This mechanism is called _call-by-value_.

The call-by-value mechanism has its limitation.  Sometimes, it is useful for a function to return more than one result.

You have seen an example before in your [Exercise 2](../exercises/ex02.md), where, for the `collatz` problem, you are supposed to find both the largest stopping time and the value with the largest stopping time.

C functions, however, can only return at most one value.  One way to get around this limitation is to use call-by-reference, the other is to use `struct`.  We will leave the discussion of `struct` for another day, so let's see how we can call non-array variables by reference.

We use call-by-reference by passing in _the address of a variable_ into a function, instead of _the value of a variable_.  Here is an example taken from the `collatz` problem.

## Example: Collatz

```C
void find_max_steps(long n, long *max_n, long *max_num_steps) {
  *max_num_steps = 0;
  *max_n = 1;
  for (long i = 1; i <= n; i += 1) {
    long num_of_steps = count_num_of_steps(i);
    if (num_of_steps >= *max_num_steps) {
      *max_n = i;
      *max_num_steps = num_of_steps;
    }
  }
}
```

The method `find_max_steps` takes in two pointers.  Inside the function, we use the deference operator `*` to modify the variable pointed to by the pointers (Lines 2, 3, 7, and 8).

To use this function, we have:

```C
int main()
{
  long n = cs1010_read_long();
  long max_num_steps;
  long max_n;
  find_max_steps(n, &max_n, &max_num_steps);
  cs1010_println_long(max_n);
  cs1010_println_long(max_num_steps);
}
```

In Line 4 above, we pass in the address of `max_n` and `max_num_steps` into `find_max_steps`.  `find_max_steps` updates both variables for us.

## Example: Swapping Two Variables

Another example of call-by-reference is a function that swaps two variables.  Here is one that swaps two `long` variables.

```C
void swap(long *a, long *b) {
  long temp = *a;
  *a = *b;
  *b = temp;
}
```

To see `swap` in action, consider:
```C
long a = 10;
long b = -4;
swap(&a, &b);
```

After calling `swap`, the value for `a` becomes -4, `b` becomes 10.

## Types of Call-by-Reference Parameters

A parameter passed as a pointer could be used in three different ways:

- The parameter could be a read-only input, and the main purpose of passing in the value is so that the function has access to the value of the pointer.  
- The parameter could be used as a vessel for the function to pass a value to the caller, similar to the parameters `max_n` and `max_num_steps` in the function `find_max_steps` above.  In this case, the value contained in the variable pointed to by the pointer does not matter.
- The parameter could be used as both input and output.  The value contained in the parameter is read inside the function, and the value is updated from inside the function.  For example, the parameters passed to `swap` above.

!!! note "Documenting parameters in CS1010"
    In CS1010, we will be using the `Doxygen` format to document our functions.  There are three types of parameters, corresponding to the three situations above: `@param[in]` is used to document a read-only parameter (note that this applies to all read-only parameters, not just pointers).  `@param[out]` is used to document an output-only parameter, and `@param[in,out]` is used to document a parameter that is both input and output.

## Problem Set
### Problem 16.1

Consider the program below:
```C
void foo(double *ptr, double trouble) {
  ptr = &trouble;
  *ptr = 10.0;
}

int main() {
  double *ptr;
  double x = -3.0;
  double y = 7.0;
  ptr = &y;

  foo(ptr, x);

  cs1010_println_double(x);
  cs1010_println_double(y);
}
```

What would be printed?
