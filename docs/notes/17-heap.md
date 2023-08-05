# Unit 17: Heap

## Learning Objectives

After completing this unit, students should:

- understand the differences between the stack and the heap
- be able to use `malloc` and `calloc` to declare dynamically-sized arrays on the heap
- understand the importance of (i) checking for NULL and (ii) freeing the memory allocated on the heap
- aware of the possibility of memory leakage and avoid common mistakes that could lead to memory leaks

## Variables on Heap

We have already seen what a call stack is and how the call stack works in [Unit 13](13-call-stack.md).  There is another important area of memory used by our programs, called the _heap_.

The memory allocation on the heap can be done automatically or manually.  For variables allocated on the heap, their lifetime is either the same as the lifetime of the _whole_ program.  An example of such a variable is a _global_ variable -- a variable that is declared _outside_ of any function and can be read from or written to anywhere in the program.  We have banned the use of global variables in CS1010 as it makes your code hard to understand or reason about:

```C
x = 1;
foo();
// { x == ?? }
```

Suppose `x` is a global variable, we cannot assert anything about the property of `x` after calling `foo`, since `x` can be modified by `foo` or any function it calls, even though we _never pass `x` into `foo`_.  This is worse than passing an array as we have seen in [Unit 16](16-call-by-reference.md)!  Now, to assert something about `x`, we need to trace through every line of the code to how `x` is updated, even if the function does not take in `x` as a parameter.

## Manual Memory Allocation / Deallocation

Allocating memory on the heap, however, is useful if we want to allocate an array dynamically, i.e., not knowing what is the size of the array when we write the program.  Often, we need an array whose size depends on the input from the user, such as reading a string or reading a sequence of numbers.  We cannot use a fixed-length array unless we know for sure that the input size is limited, and we cannot use a variable-length array, since we may get a segfault if the array size is too big for the stack.  The only viable solution is to allocate the array on the heap.

The C standard library provides a few functions related to memory allocation on the heap.  The header file for these functions is `stdlib.h`.  We are interested in `malloc` and `calloc`.

### `malloc`

`malloc` (memory allocation) is declared as:
```C
void *malloc(size_t size);
```

It takes in a parameter, `size`, which is the number of bytes of memory to be allocated, and returns a pointer to the memory allocated if successful, or `NULL` otherwise.  This is a general function so the type of pointer returned is `void *` rather than a pointer to a specific type.

### The `size_t` type

The type of `size` is `size_t`, which is an _unsigned integer type_ defined in `stdlib.h` to represent the size of a type/variable in memory. 

!!! note "Reading and writing `size_t` variables with CS1010 I/O library"
    You can read a `size_t` variable from the standard input using the CS1010 library function `cs1010_read_size_t()`.  You can similarly print a `size_t` variable with `cs1010_print_size_t` or `cs1010_println_size_t`. 

While both `size_t` and `long` are integer types, they are not compatible with each other.  Explicit casting is needed to assign the value of one type to the other.

Furthermore, one must take care when comparing between values of unsigned type `size_t` and signed type `long`.  Your code can behave unexpectedly.  Suppose `s` is a variable of type `size_t`, the comparison `s < -1` may return `true`, even though `s` is always non-negative!

It is also a common bug to write code like this:
```C
size_t i = 100;
while (i >= 0) {
	// do something to a[i]
	i -= 1;
}
```

which loops forever, since `i >= 0` is always true.

### `calloc`

The function `calloc` (clear allocation) is declared as:
```C
void *calloc(size_t count, size_t size);
```

`calloc` allocates memory for `count` items, each of `size` number of bytes, in a contiguous region in the memory and initializes all bits in this memory region to 0.  Except for the fact that `calloc` initializes the bits to 0 and `malloc` does not, `calloc(count, size)` is the same as `malloc(count * size)`.

### `sizeof`

We have seen in [Unit 5](05-first-c.md) that the number of bytes needed to represent a type depends on the platform.  Suppose we want to allocate enough memory for, say, 10 `long` values, how do we know how many bytes are needed?  A `long` can be 4 bytes on some platforms, 8 bytes on others.  For this purpose, C provides a `sizeof` operator, that returns the number of bytes needed for a type or a variable (of type `size_t`)  So, to allocate memory for 10 `long` values, we say:
```C
long *array = malloc(10 * sizeof(long));
```

## Memory Deallocation

Even though the memory available on the heap is larger than the stack, it is not unlimited, and therefore we should still use the memory judiciously.   In particular, after we are done using the memory allocated to us, we should call the method `free`, passing in the pointer the memory region allocated, to have the memory region deallocated and returned to the OS to be reused by others.

A common bug is for a programmer to access a memory that has been freed or free an allocated memory region more than once.  This would cause strange behaviors -- possibly crashing in random places if we access memory that is being used by others.  It is a good practice to set the pointer to NULL after `free`-ing the memory region so that we do not accidentally use it.

```
free(buffer);
buffer = NULL;
```

Another common bug is for programmers to request memory via `malloc` or related functions, but forgot to `free` it back to the OS.  As a result, as the program runs, it starts to hog the memory and the system will become slower and slower.   This bug is known as _memory leak_.

Another possible bug is for programmers to change the pointer to the region of memory allocated.  For instance,
```
long *buffer = calloc(how_many, sizeof(long));
buffer = cs1010_read_long_array(how_many);
```

After we execute the code such as the above, the pointer `buffer` will point to something new, and there is no longer a pointer pointing to allocate memory.  The memory allocated becomes unreachable, and therefore we can no longer free it!

!!! note "Freeing memory returned by CS1010 I/O calls"
    In CS1010, from now on, you are to make sure that memory that is allocated via `malloc` and related functions are `free` after it is used, including those allocated in the CS1010 I/O library.  The [API documentation](../guides/library.md) tells you which values returned by the library should be deallocated by the caller using `free`.

## Memory Reallocation

If we allocate an array on the heap, we can even change the size of the array dynamically during run-time.  For instance, there could be cases where we don't know the size of the array we need.  We can allocate an array, say, of size `n`, at the beginning of our program.  As we populate the array, we may realize that `n` is too small, and we need a bigger array.  We can use the method `realloc` to reallocate our array to a larger size as needed.  Interested students can read the man page for `realloc` for more details. 

## CS1010 I/O Library

To wrap up this unit, we will look at the CS1010 library functions that help us read in an array of either `long` values or `double` values.  These functions, `cs1010_read_long_array` or `cs1010_read_double_array` take in a parameter, which is the number of elements to read (of type `size_t`) and it returns a pointer to the array allocated within the function.

A typical usage is as follows: we first read the size of the array `size` with `cs1010_read_size_t()`, then use `cs1010_read_long_array(size)` to read the array.

Here is the incomplete version:
```C title="Incomplete code without NULL checks and deallocation"
size_t num_of_students = cs1010_read_size_t();
long *marks;
marks = cs1010_read_long_array(num_of_students);
for (size_t i = 0; i < num_of_students; i += 1) {
  cs1010_println_long(marks[i]);
}
```

This should be straightforward enough.  There are, however, two cases to consider.  What if the OS failed to allocate the memory for our array?  In this case, marks would be `NULL` and access `marks[i]` would cause a segfault. Second, we must return the memory allocated to us back to the OS once we are done.  To let go of this memory, we call the function `free`.  The complete code looks like this:

```C
size_t num_of_students = cs1010_read_size_t();
long *marks;
marks = cs1010_read_long_array(num_of_students);
if (marks == NULL) {
  // signal error and return
}
for (size_t i = 0; i < num_of_students; i += 1) {
  cs1010_println_long(marks[i]);
}

// do other things to marks
  :

free(marks);
```

The CS1010 I/O library, internally, allocates memory on the heap so that we can read in words, lines, or arrays of arbitrary length.

The following shows an example of how `cs1010_read_long_array` is implemented with `calloc`.

```C
long* cs1010_read_long_array(size_t how_many)
{
  long *buffer = calloc(how_many, sizeof(long));
  if (buffer == NULL) {
    return NULL;
  }

  for (size_t i = 0; i < how_many; i += 1) {
    buffer[i] = cs1010_read_long();
  }

  return buffer;
}
```


## Problem Set
### Problem Set 17.1

Draw the call stack and the heap, showing what happened when we run the following code:

```C
void foo(long *y, long *z)
{
  y[0] = -7;
  y[1] = -8;
  z[0] = 4;
  z[1] = 5;
}


int main()
{  
  long y[2] = {1, 2};
  long *z = calloc(2, sizeof(long));

  z[0] = y[0];
  z[1] = y[1];

  foo(y, z);
}
```
