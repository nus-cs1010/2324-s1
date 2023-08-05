# Unit 18: Characters and Strings

## Learning Objectives

After this unit, students should:

- understand that a string is an array of chars
- know that a string always ends with a `char` with value 0 (or `'\0'`)
- be aware of the different types of special chars
- understand what the empty string represents
- be able to use the CS1010 library functions to read strings and arrays of strings

## The `char` type

Recall that the `char` type is of size 8 bits. The ASCII code maps each bit sequence to character the letters, digits, punctuations, and some non-visible characters such as null, escape, newline, beep, etc.  To denote a character literal, we use a pair of single quotes `'`.  For instance, we can write something like this:

```C
char x = 'A';
```

Further, recall that the type of a variable determines how we interpret a bit sequence.  Given the same bit sequence, we can interpret it differently if we assign it to a different type.  If we call

```C
char x = 'A';
putchar(x);
cs1010_println_long(x);
```

We will see that character `A` is first printed, followed by its numeric value `41`.  We can use this property to manipulate the `char` variable with arithmetic operations.  For instance, 

```C
char x = 'A';
putchar(x + 1);
```

would print `B`.

## Special Characters

The use of the backslash `\` creates an escape sequence that can be used to denote characters that would otherwise not be visible on the screen.  For instance, `'\n'` refers to the newline character, `'\t'` refers to the tab character, `'\a'` refers to the beep character.

Furthermore, since we already use `\` to indicate the escape sequence, `'` to indicate a character, and `"` to indicate a string, to use these characters, we need to "escape" them -- we use `'\\'` for the backslash character, `'\''`, the single quote character, and the `'\"'` double-quote character.

## String

We have seen strings as a sequence of characters stored in double quotes, e.g., `"hello!"`.  Note how we distinguish between a string and a `char` by the quotes used.  String uses double quotes `"`; a `char` uses a single quote `'`.  

In C, a string is nothing more than just an array of `char` values. The only thing special about a string is that it _always_ ends with a `0` value (note: not character `'0'` which has a value of 48, but the value 0).  The ASCII character with the value 0 is called the _null_ character, written as `'\0'`.  We, therefore, refer to strings in C as null-terminated strings.  Note that we distinguish the null character from the `NULL` pointer: the former is of type `char` and is always 0; the latter is of type `void *` and may or may not be 0.

The following two ways of initializing a string are thus equivalent:
```C
char hello1[7] = {'h', 'e', 'l', 'l', 'o', '!', '\0'};
char hello2[7] = "hello!";
```

The second way is more easily readable.

We can also skip the size of the array, as mentioned above, or, more commonly, use a `char *` type for a variable storing a constant string.
```C
char hello3[] = "hello!";
char *hello4 = "hello!";
```

## String Literals

A _string literal_ refers to a string written between two `"` characters, such as `"Hello world!"`.  Such a string is still internally stored as an array of `char` values.  The location these arrays are stored depends on the platform, but usually, they are stored in a read-only region of the memory called the `text` region.  These strings are not meant to be modified.  Hence, trying something like this:

```C
char *str1 = "Hello!";
str1[5] = '.';
```

is not allowed and would crash your program.

The following, however, is OK:
```C
char str2[7] = "Hello!";
str2[5] = '.';
```

The difference between the two is that `str1` points to a read-only region in the memory, while `str2` contains a copy of the string on the stack.

## Empty String

We commonly use the empty string `""` to indicate a special condition or to initialize a string variable, where appropriate.  The empty string is nothing but an array where the 0-th element is `'\0'`.

## Common Bugs

A very common bug for beginners to C programming is to forget about the terminating `'\0'` char in a string.  For instance, suppose you want to copy one string to another:

```C
    char src[6] = "hello!"
    char dst[6];
    for (long i = 0; i < 6; i += 1) {
        dst[i] = src[i];
    }
```

The string variable `dst` is not terminated and C would treat whatever follows the memory location as part of the string!

The other common bug is creating an array that is too small for the string.  Suppose you use the function `strcpy` provided by C to copy the string instead.

```C
    char src[6] = "hello!"
    char dst[6];
    strcpy(dst, src);
```

`strcpy` would properly copy the characters `'h'`, `'e'`,  .. `'!'`, AND the terminating `'\0'`.  That's a total of 7 characters.  The code, however, only allocates the space of 6 characters for `dst`.  So overflow occurs and your program might crash.

## CS1010 I/O Library

The CS1010 I/O library provides two functions, one to read a word (separated by white-space characters) and the other to read a line (separated by a newline character).  They are `cs1010_read_word()` and `cs1010_read_line()` respectively.  We can also read multiple words and multiple lines with `cs1010_read_word_array()` and `cs1010_read_line_array()`.  The results are stored in an array of strings.

The arrays and strings returned by these functions need to be deallocated by the caller.  Particularly, note that when an array of strings is returned by both `cs1010_read_word_array()` and `cs1010_read_line_array()`, _both_ the strings and the array holding the strings all need to be freed.

Typical usage is as follows:

```C
size_t k = cs1010_read_size_t();
char** lines = cs1010_read_line_array(k);
if (lines == NULL) {
  // Deal with error
  return;
}
// Do something with array lines
  :
  :
// free each line
for (size_t i = 0; i < k; i += 1) {
  free(lines[i]);
}
// free the array
free(lines);
```
