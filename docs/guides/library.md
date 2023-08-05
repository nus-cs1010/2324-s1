# The CS1010 I/O Library

To help students get started with C programming without worrying too much about the details and pitfalls of using `printf` and `scanf`, we provide a simple-to-use library to read and write integers, floating point numbers, and strings.  

The libraries are pre-installed in [CS1010 programming environments](environments.md), with `cs1010.h` located under `~cs1010/include` and `libcs1010.a` located under `~cs1010/lib`.  During practical exams, a copy will be provided under your exam home directory (under `~/include` and `~/lib`).

## Using the Library

### Header

To use the CS1010 I/O library, you should `#include` the file `cs1010.h`, like this:

```C
#include "cs1010.h"
```

at the top of your C program.

### Linking

The CS1010 I/O library is provided as the file `libcs1010.a`.  The `Makefile` provided by CS1010 is already configured to link to this library.  When you run `make` to compile your code, this library is linked by default.

If you wish to manually compile your code, you need to link to the library, by supplying the argument `-lcs1010` to `clang`.  Usually, you also need to specify where you can find `cs1010.h` with the `-I` flag, and `libcs1010.a` with the `-L` flag.  For instance, in the CS1010 programming environment, you would need to compile using the command line:
```
clang -I ~cs1010/include -L ~cs1010/lib hello.c -lcs1010
```

## Reading of a Single Value

The CS1010 library supports reading of a single numeric value of type `long`, `double`, and `size_t`, as well as reading of strings (both space-separated _words_ and newline-separated _lines_) from the standard input. 

### `long cs1010_read_long()`<br>
Returns a `long` value from the standard input.  An error message will be printed (to `stderr`) if the input sequence is not a valid `long` value -- in which case the value `LONG_MAX` will be returned.  Example:
```C
long year = cs1010_read_long();
```

### `double cs1010_read_double()`<br>
Returns a `double` value from the standard input.  An error message will be printed (to `stderr`) if the input sequence is not a valid `double` value -- in which case the value `DBL_MAX` will be returned. Example:
```C
double cap = cs1010_read_double();
```

###  `size_t cs1010_read_size_t()`<br>
Returns a `size_t` value from the standard input.  An error message will be printed (to `stderr`) if the input sequence is not a valid `size_t` value -- in which case the value `0` will be returned.  Example:
```C
size_t size = cs1010_read_size_t();
```

### `char* cs1010_read_word()`<br>
Returns a `char *` pointing to the next white-space-separated string from the standard input.  A white-space character is defined based on the standard C function `isspace()` and includes the space ` `, tab `\t`, and newline `\n` character.  Returns `NULL` if there is an error.  If the returned value is non-`NULL`, the caller is responsible for freeing the memory allocated by calling `free`.

```C title="Usage example of cs1010_read_word()"
char* word = cs1010_read_word();
if (word == NULL) {
  // Deal with error
} else {
  // Do something with word
  :
  :
  free(word);
}
```

Suppose the input is:
```
hello world
1234567
```

Calling `cs1010_read_word()` would return the string `"hello"`.

### `char* cs1010_read_line()`<br>
Returns a `char *` pointing to the next new-line-separated string from the standard input.  The string returns from `cs1010_read_line()` includes the newline character (if one is found).
Returns `NULL` if there is an error.  If the returned value is non-`NULL`, the caller is responsible for freeing the memory allocated by calling `free`.

```C title="Usage example of cs1010_read_line()"
char* line = cs1010_read_line();
if (line == NULL) {
  // Deal with error
} else {
  // Done something with line
  :
  :
  free(line);
}
```

Suppose the input is:
```
hello world
1234567
```

Calling `cs1010_read_line()` would return the string `"hello world\n"`.

## Reading of Multiple Values

The CS1010 library also supports the reading of multiple values and returning the values in an array.

### `long* cs1010_read_long_array(size_t k)`<br>
Returns `k` numbers of `long` values read from the standard input stored in an array.  An error message will be printed (to `stderr`) for each input that is not a valid `long` value -- in which case the value `LONG_MAX` will be populated in the corresponding array element.  Returns `NULL` if there is a memory allocation error.  If the returned value is non-`NULL`, the caller is responsible for freeing the memory allocated by calling `free`.
```C
size_t k = cs1010_read_size_t();
long* values = cs1010_read_long_array(k);
if (values != NULL) {
  // Do something with array values
   :
   :
  free(values);
}
```

### `double* cs1010_read_double_array(size_t k)`<br>
Returns `k` numbers of `double` values read from the standard input stored in an array.  An error message will be printed (to `stderr`) for each input that is not a valid `double` value -- in which case the value `DBL_MAX` will be populated in the corresponding array element.
Returns `NULL` if there is a memory allocation error.  If the returned value is non-`NULL`, the caller is responsible for freeing the memory allocated by calling `free`.
```C
size_t k = cs1010_read_size_t();
double* values = cs1010_read_double_array(k);
if (values != NULL) {
  // Do something with array values
   :
   :
  free(values);
}
```

### `char** cs1010_read_word_array(size_t k)`<br>
Returns `k` white-space-separated words read from the standard input stored in an array.  The notion of "word" is the same as `cs1010_read_word()`.
Returns `NULL` if there is a memory allocation error.  If the returned value is non-`NULL`, the caller is responsible for freeing the memory allocated for each word and the whole array by calling `free`.

```C title="Usage example for cs1010_read_word_array"
size_t k = cs1010_read_size_t();
char** words = cs1010_read_word_array(k);
if (words == NULL) {
  // Deal with error
} else {
  // Do something with the array words
   :
   :
  for (size_t i = 0; i < k; i += 1) {
    free(words[i]);
  }
  free(words);
}
```

Suppose the input is:
```
hello world
1234567
```

Calling `cs1010_read_word_array(2)` would return the array `{"hello", "world"}`.

### `char** cs1010_read_line_array(size_t k)`<br>
Returns `k` new-line-separated words read from the standard input stored in an array.  The notion of a line is the same as `cs1010_read_line()`.
Returns `NULL` if there is a memory allocation error.  If the returned value is non-`NULL`, the caller is responsible for freeing the memory allocated for each line and the whole array by calling `free`.

```C title="Usage example for cs1010_read_line_array"
size_t k = cs1010_read_size_t();
char** lines = cs1010_read_line_array(k);
if (lines == NULL) {
  // Deal with error
} else {
  // Do something with the array lines
    :
    :
  for (size_t i = 0; i < k; i += 1) {
    free(lines[i]);
  }
  free(lines);
}
```

Suppose the input is:
```
hello world
1234567
```

Calling `cs1010_read_line_array(2)` would return the array `{"hello world\n", "1234567\n"}`.

## Printing of a Single Value

The CS1010 library provides a few convenient functions to format and print `long` and `double` values to the standard output.

### `void cs1010_print_long(long value)` and `void cs1010_println_long(long value)`<br>
Print `value` to the standard output (with `printf` format `%ld`).  
The `cs1010_println_long` version prints a newline after the value.
```C
  long x;
    :
  cs1010_print_long(x);
```

### `void cs1010_print_double(double value)` and `void cs1010_println_double(double value)`<br>
Print `value` to the standard output (with `printf` format `%.4f`).
The `cs1010_println_double` version prints a newline after the value.
```C
  double x;
    :
  cs1010_println_double(x);
```

### `void cs1010_print_size_t(size_t value)` and `void cs1010_println_size_t(size_t value)`<br>
Print `value` to the standard output (with `printf` format `%zu`).
The `cs1010_println_size_t` version prints a newline after the value.
```C
  size_t x;
    :
  cs1010_println_size_t(x);
```

### `void cs1010_print_string(char *str)` and `void cs1010_println_string(char *str)`<br>
Print a given string `str` to the standard output.  These functions are provided for completeness and is a simple wrapper around `printf(str)` and `printf("%s\n", str)` repsectively.
```C
  cs1010_println_string("hello world!");
```

!!! tip "Printing single character"
    There is no `cs1010_print_char` method.  You can use `putchar` from the C standard library
	for this purpose.

### `void cs1010_print_pointer(void *ptr)` and `void cs1010_println_pointer(void *ptr)`<br>
Print a given pointer `ptr` to the standard output in decimal format.
```C
  long *x;
  cs1010_println_pointer(&x);
```

### `void cs1010_print_bool(bool x)` and `void cs1010_println_bool(bool x)`<br>
Print a given boolean value `x` to the standard output as either `true` or `false`.
```C
  bool x = true;
  cs1010_println_bool(x);
```

## Clearing screen

The CS1010 library provides a function to clear your screen. 
```C
  cs1010_clear_screen();
```


# Optional: Installing the Library

If you want to install the libraries on your computer for purposes other than CS1010, you can do the following:

1. To get an updated copy of the library, clone it from its git repo on GitHub with the command:

```
git clone https://github.com/nus-cs1010/libcs1010.git
```

It is recommended you do this in your home directory.

You should see an output similar to:
```
Cloning into 'libcs1010'...
remote: Counting objects: 6, done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 6 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (6/6), done.
```

After that, you should see a subdirectory `libcs1010` created in your current directory.  Inside, there should be a file called `Makefile`, and two subdirectories called `include` and `src`.  

2. To compile the library, run

```
make
```

This should compile the file `src/cs1010.c` and create a static C library named `libcs1010.a` under the `lib` directory.
