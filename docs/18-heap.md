# Unit 18: Heap

## Learning Objectives

After completing this unit, students should:

- understand the differences between the stack and the heap
- be able to use `malloc` and `calloc` to declare dynamically-sized arrays on the heap
- understand the importance of (i) checking for NULL and (ii) free the memory allocated on the heap
- aware of the possibility of memory leakage and avoid common mistakes that could lead to memory leaks

## Auto Variables
We have already seen what a call stack is and how the call stack works in [Unit 13](13-call-stack.md).  There is another important area of memory used by our programs, called the _heap_.

Recall that a variable allocated on the stack has two properties:

- Its lifetime is the same as the lifetime of the function the variable is declared in.
- The memory allocation and deallocation are automatic.  

For stack, the memory is allocated automatically when the function is called and deallocated automatically as soon as the function exits.  For this reason, such a variable is sometimes called _automatic_ variable, or _auto_ variable for short.

## Variables on Heap

The memory allocation on the heap can be done automatically or manually.  For variables allocated on the heap, its lifetime is either the same as the lifetime of the _whole_ program.  An example of such a variable is a _global_ variable -- a variable that is declared _outside_ of any function and can be read or write anywhere in the program.  We have banned the use of global variables in CS1010.  Using a global variable makes your code hard to understand or reason about:

```C
x = 1;
foo();
// { x == ?? }
```

Suppose `x` is a global variable, we cannot assert anything about the property of `x` after calling `foo`, since `x` can be modified by `foo` or any function it calls, even though we _never pass `x` into `foo`_.  This is worse than passing an array as we have seen in [Unit 17](17-call-by-reference.md)!  Since now, to assert something about `x`, we need to trace through every line the code to how `x` is updated, even if the function does not take in `x` as a parameter.

## Manual Memory Allocation / Deallocation

Allocating memory on the heap, however, is useful if we want to allocate an array dynamically, i.e., not knowing what is the size of the array when we write the program.  Often, we need an array whose size depends on the input from the user, such as reading a string or reading a sequence of numbers.  We cannot use fixed-length array unless we know for sure that the input size is limited, and we cannot use a variable-length array, since we may get a segfault if the array size is too big for the stack.  The only viable solution is to allocate the array on the heap.

The C standard library provides a few functions related to memory allocation on the heap.  The header file for these functions is `stdlib.h`.  We are interested in `malloc` and `calloc`.  `malloc` (memory allocation) is declared as:
```C
void *malloc(size_t size);
```

It takes in a parameter, `size`, which is the number of bytes of memory to be allocated and returns a pointer to the memory allocated if successful, or `NULL` otherwise.  This is a general function so the type of pointer returned is `void *` rather than a pointer to a specific type.  The type of `size` is `size_t`, which is a type defined in `stdlib.h` to represent the number of bytes in memory.

The function `calloc` (clear allocation) is declared as:
```C
void *calloc(size_t count, size_t size);
```

`calloc` allocates memory for `count` items, each of `size` number of bytes, in a contiguous region in the memory and initializes all bits in this memory region to 0.  Except for the fact that `calloc` initializes the bits to 0, `calloc(count, size)` is the same as `malloc(count * size)`.

We have seen in [Unit 5](05-first-c.md) that the number of bytes needed to represent a type depends on the platform.  Suppose we want to allocate enough memory for, say, 10 `long` values, how do we know how many bytes are needed?  A `long` can be 4 bytes on some platforms, 8 bytes on others.  For this purpose, C provides a `sizeof` operator, that returns the number of bytes needed for a type or a variable.  So, to allocate memory for 10 `long` values, we say:
```C
long *array = malloc(10 * sizeof(long));
```

!!! warning "Casting to size_t"
    We need to cast `long` arguments into `malloc` and `calloc` as `size_t` 
	to suppress warnings from `clang`.

The CS1010 I/O library, internally, allocates memory on the heap so that we can read in words, lines, or arrays of arbitrary length.

The following shows the example of how `cs1010_read_long_array` is implemented with `calloc`.

```C
long* cs1010_read_long_array(long how_many)
{
  long *buffer = calloc((size_t)how_many, sizeof(long));
  if (buffer == NULL) {
    return NULL;
  }

  for (long i = 0; i < how_many; i += 1) {
    buffer[i] = cs1010_read_long();
  }

  return buffer;
}
```

!!! warning "Casting to size_t"
    We need to cast `long` arguments into `malloc` and `calloc` as `size_t` 
	to suppress warnings from `clang`.

## Memory Deallocation

Even though the memory available on the heap is larger than the stack, it is not unlimited, and therefore we should still use the memory judiciously.   In particular, after we are done using the memory allocated to us, we should call the method `free`, passing in the pointer the memory region allocated, to have the memory region deallocated, returned to the OS to be reused by others.

A common bug is for a programmer to access a memory that has been freed.  This would cause strange behaviors, possible crashes in random places since we will be accessing memory that is being used by others.  It is a good practice to set the pointer to NULL after `free`-ing the memory region so that we do not accidentally use it.

```
free(buffer);
buffer = NULL;
```

Another common bug is for programmers to request memory via `malloc` or related functions, but forgot to `free` it back to the OS.  As a result, as the program runs, it starts to hog the memory and the system will become slower and slower.   This bug is known as _memory leak_.  

Another possible bug is for programmers to change the pointer to the region of memory allocated.  For instance,
```
long *buffer = calloc((size_t)how_many, sizeof(long));
buffer = cs1010_read_long_array(how_many);
```

After we execute the code such as the above, the pointer `buffer` will point to something new, and there is no longer a pointer pointing to allocate memory.  The memory allocated becomes unreachable, and therefore we can no longer free it!

In CS1010, from now on, you are to make sure that memory that is allocated via `malloc` and related functions are `free` after it is used, including those allocated in the CS1010 I/O library.  The [API documentation](library.md) tells you what are the values returned by the library that should be deallocated by the caller via `free`.

## Problem Set
### Problem Set 18.1

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

### Problem Set 18.2

Read the man page for the function `realloc` and explain what does it do.  Can you come up with a situation where it could be useful?
